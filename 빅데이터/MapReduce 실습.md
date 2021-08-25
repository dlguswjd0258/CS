# :world_map: MapReduce 실습



## 코드

#### wordcount.java

- Map

  ```java
  public static class TokenizerMapper extends Mapper<입력 key 타입, 입력 value 타입, 출력 key 타입, 출력 value 타입>{
      
      // 출력값에 사용되는 변수, 상수
      private final static IntWritable one = new IntWritable(1);
      private Text word = new Text();
  
      // map function (Context는 고정된 인자)
      public void map(Object key, Text value, Context context)
          throws IOException, InterruptedException {
  
          // value.toString() : get a line
          StringTokenizer itr = new StringTokenizer(value.toString());
          while ( itr.hasMoreTokens() ) {
              word.set(itr.nextToken());
  
              // emit a key-value pair
              context.write(word,one);
          }
      }
  }
  ```

  

- Reduce

  ```java
  public static class IntSumReducer
      extends Reducer<입력 key 타입, 입력 value 타입, 출력 key 타입, 출력 value 타입> {
  
      // 출력값에 사용되는 변수
      private IntWritable result = new IntWritable();
  
      // key : a disticnt word
      // values :  Iterable type (data list)
      public void reduce(Text key, Iterable<IntWritable> values, Context context) 
          throws IOException, InterruptedException {
  
          int sum = 0;
          for ( IntWritable val : values ) {
              sum += val.get();
          }
          result.set(sum);
          context.write(key,result);
      }
  }
  ```



- main

  ```java
  public static void main(String[] args) throws Exception {
      Configuration conf = new Configuration();
      String[] otherArgs = new GenericOptionsParser(conf,args).getRemainingArgs();
      if ( otherArgs.length != 2 ) {
          System.err.println("Usage: <in> <out>");
          System.exit(2);
      }
      Job job = new Job(conf,"word count"); // (job 작성, 설명(안써도 된다))
      job.setJarByClass(Wordcount.class); // (job 수행할 class), 대소문자 주의
  
      // hadoop에게 작성한 Map과 Reudce class를 알려준다
      job.setMapperClass(TokenizerMapper.class);
      job.setReducerClass(IntSumReducer.class);
  
      // Mapr과 Reduce의 output의 key와 value 타입 지정
      // Map과 Reduce의 output이 같다면 Map output에 대한 지정 생략
      job.setOutputKeyClass(Text.class);
      job.setOutputValueClass(IntWritable.class);
  
      // 동시에 수행되는 reducer의 개수 설정
      job.setNumReduceTasks(2);
  
      // set input and output directories
      // input과 output 파일을 저장할 path 설정
      FileInputFormat.addInputPath(job,new Path(otherArgs[0]));
      FileOutputFormat.setOutputPath(job,new Path(otherArgs[1]));
      
      // 실행
      System.exit(job.waitForCompletion(true) ? 0 : 1 );
  }
  ```

  

#### Mapper나 Reducer에 값을 넘겨주기

- main

  ```java
  public static void main(String[] args) throws Exception {
      // ...
      Job job = new Job(conf,"word count");
      // ...
      job.setOutputValueClass(IntWritable.class);
  
      // 넘겨줄 인자 setting
     	Configuration config = job.getConfiguration();
      config.set("name", "Shim"); // name의 값은 "Shim"
      config.setInt("one", 1);
      config.setFloat("point_five", (float)0.5);
      
      //..
      
      FileInputFormat.addInputPath(job,new Path(otherArgs[0]));
      FileOutputFormat.setOutputPath(job,new Path(otherArgs[1]));
      System.exit(job.waitForCompletion(true) ? 0 : 1 );
  }
  ```

  

- Mapper나 Reducer의 setup

  ```java
  public static class IntSumReducer
      extends Reducer<입력 key 타입, 입력 value 타입, 출력 key 타입, 출력 value 타입> {
  	// ...
      
     	private String name;
      private int one;
      private float point;
      
      // setup 함수
      protected void setup(Context context) throws IOException, InterruptedException {
          Configuration config = context.getConfiguration();
          name = config.get("name", "Kim"); // (값 가져오기, default 값)"
          one = config.getInt("one", 1);
          point = config.getFloat("point_five", (float)0.5);
      }
  }
  ```

  



## 새로운 MapReduce Code 컴파일하는 법

- 소스 코드 파일을 Project/src/ 디렉토리에 넣는다

- Project/src 디렉토리에 있는 Driver.java 파일에 아래 라인을 넣는다.

  ```java
  pgd.addClass("program name", class name, "description");
  ```

- 컴파일을 하려면 Project 디렉토리에서 ant라고 타입하고 수행

  ```
  $ ant
  ```

