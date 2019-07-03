# javamail使用
---
#### maven引用
spring boot 中包含有javamail直接引入
>
    <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-mail</artifactId>
     </dependency>

#### properties配置
>
\#发送者邮箱pop  
spring.mail.host=smtp.163.com  
\#发送者邮件地址  
spring.mail.username=qyngchen@163.com  
\#邮件发送者授权码  
spring.mail.password=qy13148  
\#接受者邮件  
recipient.username=969515689@qq.com  
\#配置信息  
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.mstp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
server.port=9093
 
#### 代码实现
    @Autowired
    private JavaMailSender mailSender;

    @Value("${spring.mail.username}")
    private String sender;

    @Value("${recipient.username}")
    private String recipient;  
  
    SimpleMailMessage mailMessage = new SimpleMailMessage();  
    mailMessage.setFrom(sender);  
    mailMessage.setTo(recipient);  
    mailMessage.setSubject("测试邮件发送");  
    mailMessage.setText("来自163邮箱的测试邮件");  
    mailSender.send(mailMessage);
#### 源代码地址
https://github.com/qyngchen/sbtest.git
