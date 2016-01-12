---
layout: post
title: "Hadoop — MapReduce实例"
description: "初识MapReduce HelloWorld"
category: Hadoop
tags: [Hadoop, MapReduce]
---
{% include JB/setup %}


解析单词实例
----------
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    import java.io.IOException;
    import java.util.StringTokenizer;

    public class WordCount {

        //继承Mapper接口，设置map的输入类型为<Object, Text>
        //输出类型为<Text, IntWritable>
        public static class TokenizerMapper extends Mapper<Object, Text, Text, IntWritable> {
            //one表示单词出现1次
            public final static IntWritable one = new IntWritable(1);
            //word用于存储切下的词
            private Text word = new Text();

            public void map(Object key, Text value, Context context) throws IOException, InterruptedException{
                StringTokenizer itr = new StringTokenizer(value.toString()); //对输入的行切词
                while (itr.hasMoreTokens()) {
                    word.set(itr.nextToken());
                    context.write(word, one);
                }
            }
        }

        //继承Reduce接口，设置Reduce的输入类型为<Text, IntWritable>
        //输出类型为<Text, IntWritable>
        public static class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
            //result记录单词频数
            private IntWritable result = new IntWritable();

            public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException{
                int sum = 0;
                //对获取的<key, value-list>计算value的和
                for (IntWritable val : values) {
                    sum += val.get();
                }
                //将频数设置到result中
                result.set(sum);
                context.write(key, result);
            }
        }

        public static void main(String[] args) throws Exception {
            Configuration conf = new Configuration();

            //检查运行命令
            String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
            if (otherArgs.length != 2) {
                System.err.println("Usage: wordcount <in><out>");
                System.exit(2);
            }
            //配置作业名
            Job job = new Job(conf, "wordcount");
            //配置作业的各个类
            job.setJarByClass(WordCount.class);
            job.setMapperClass(TokenizerMapper.class);
            job.setCombinerClass(IntSumReducer.class);
            job.setReducerClass(IntSumReducer.class);
            job.setOutputKeyClass(Text.class);
            job.setOutputValueClass(IntWritable.class);
            FileInputFormat.addInputPath(job, new Path(args[0]));
            FileOutputFormat.setOutputPath(job, new Path(args[1]));
            System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }


运行MapReduce
---------

创建一个目录classes,用于存放编译后的class文件

`mkdir classes`

编译java文件

`javac -classpath ~/woft/hadoop-1.2.1/hadoop-core-1.2.1.jar:/Users/vivian/woft/hadoop-1.2.1/lib/commons-cli-1.2.jar -d classes WordCount.java`

打包

`jar -cvf wordcount.jar -C classes/ .`

在集群上创建输入文件夹 (目录创建在默认用户根目录下 可以设置）

`~/woft/hadoop-1.2.1/bin/hadoop dfs -mkdir input`

本地创建输入文件

`mkdir input; file01 file02`

上传本地目录下的文件到集群上的input目录下

`~/woft/hadoop-1.2.1/bin/hadoop dfs -put ./input/file0* input`

运行WordCount程序

`~/woft/hadoop-1.2.1/bin/hadoop jar wordcount.jar WordCount input output`

----------------------------
