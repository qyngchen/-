# java easy-rule 
giithub:https://github.com/j-easy/easy-rules
---
## 概念
easy-rule,规则引擎的一种，可以使用构建一个简单的规则引擎。  
您只需要创建一组包含条件和操作的对象，将它们存储在集合中，  
然后运行它们以评估条件并执行操作。
##优点
- 轻量级库和易于学习的API
- 基于POJO的开发与注释编程模型
- 用于定义业务规则并使用Java轻松应用它们的有用抽象
- 能够从原始规则创建复合规则
- 使用表达式语言定义规则的能力（如MVEL和SpEL）

---
## rule
### rule的实现方式
#### 方式一：实现Rule接口(RuleBuilder API)，并实现其evaluate和execute方法
jar包位置：easy-rules-core-3.1.0-sources.jar!/org/jeasy/rules/api/Rule.java
源码：

    /**
     * 可以由规则引擎触发的规则的抽象.
     *
     * 规则在Rules类型的规则集中注册，其中名称具有唯一.
     *
     * @author Mahmoud Ben Hassine (mahmoud.benhassine@icloud.com)
     */
    public interface Rule extends Comparable<Rule> {
    
        /**
         * 默认规则名称.
         */
        String DEFAULT_NAME = "rule";
    
        /**
         * 默认规则描述.
         */
        String DEFAULT_DESCRIPTION = "description";
    
        /**
         * 默认规则优先级.
         */
        int DEFAULT_PRIORITY = Integer.MAX_VALUE - 1;
    
        /**
         * 获取规则名称.
         * @return the rule name
         */
        String getName();
    
        /**
         * 获取规则描述.
         */
        String getDescription();
    
        /**
         * 获取规则优先级.
         */
        int getPriority();
    
        /**
         * 规则条件：此方法封装规则的条件.
         */
        boolean evaluate(Facts facts);
    
        /**
         * 规则操作：此方法封装规则的操作.
         */
        void execute(Facts facts) throws Exception;
    
    }

---
#### 方式二：使用@Rule注解修饰POJO
    @Rule
    public class RuleTest {
    
        private int input;
    
        public void setInput(Integer input) {
            this.input = input;
        }
    
        /**
         * 判断是否命中规则
         */
        @Condition
        public Boolean isFlag() {
    
            return true;
        }
    
        /**
         * 命中规则之后采取的操作
         */
        @Action
        public void doSomething() {
    
        }
    
        /**
         * 优先级
         */
        @Priority
        public int priority() {
            return 1;
        }
    }
- @Rule可以标注name和description属性，每个rule的name要唯一，如果没有指定，则RuleProxy则默认取类名
- @Condition是条件判断，要求返回boolean值，表示是否满足条件
- @Action标注条件成立之后触发的方法
- @Priority标注该rule的优先级，默认是Integer.MAX_VALUE - 1，值越小越优先

---
## facts
### 概念
1. 规则的使用者，被规则的对象
2. 内部实现为一个HashMap<String,Object>
### 规则
1. facts 名称唯一，不为null
2. 任何的java object 都可以作为一个facts
### 定义
    Facts facts = new Facts();
    facts.put("rain", true);
## rules engine
### 创建规则引擎
3.1版本后easy rule提供了俩种rulesEngine接口实现  

- DefaultRulesEngine:
- InferenceRulesEngine:
### 应用
    RulesEngine rulesEngine = new DefaultRulesEngine();
    // or
    RulesEngine rulesEngine = new InferenceRulesEngine();
    //使用 传入自定义的规则和facts
	rulesEngine.fire(rules, facts);
### 规则引擎参数

参数|类型|需求|默认
-|-|-|-
rulePriorityThreshold | int | no | MAXINT
skipOnFirstAppliedRule|boolean|no|false
skipOnFirstFailedRule|boolean|no|false
skipOnFirstNonTriggeredRule|boolean|no|false