# java easy-rule 
giithub:https://github.com/j-easy/easy-rules
---
##概念
easy-rule,规则引擎的一种，可以使用构建一个简单的规则引擎。  
您只需要创建一组包含条件和操作的对象，将它们存储在集合中，  
然后运行它们以评估条件并执行操作。
##优点
- 轻量级库和易于学习的API
- 基于POJO的开发与注释编程模型
- 用于定义业务规则并使用Java轻松应用它们的有用抽象
- 能够从原始规则创建复合规则
- 使用表达式语言定义规则的能力（如MVEL和SpEL）
##实现方式
####方式一：实现Rule接口(RuleBuilder API)，并实现其evaluate和execute方法
jar包位置：easy-rules-core-3.1.0-sources.jar!/org/jeasy/rules/api/Rule.java
源码：

    /**
     * Abstraction for a rule that can be fired by the rules engine.
     *
     * Rules are registered in a rule set of type <code>Rules</code> in which they must have a <strong>unique</strong> name.
     *
     * @author Mahmoud Ben Hassine (mahmoud.benhassine@icloud.com)
     */
    public interface Rule extends Comparable<Rule> {
    
        /**
         * Default rule name.
         */
        String DEFAULT_NAME = "rule";
    
        /**
         * Default rule description.
         */
        String DEFAULT_DESCRIPTION = "description";
    
        /**
         * Default rule priority.
         */
        int DEFAULT_PRIORITY = Integer.MAX_VALUE - 1;
    
        /**
         * Getter for rule name.
         * @return the rule name
         */
        String getName();
    
        /**
         * Getter for rule description.
         * @return rule description
         */
        String getDescription();
    
        /**
         * Getter for rule priority.
         * @return rule priority
         */
        int getPriority();
    
        /**
         * Rule conditions abstraction : this method encapsulates the rule's conditions.
         * <strong>Implementations should handle any runtime exception and return true/false accordingly</strong>
         *
         * @return true if the rule should be applied given the provided facts, false otherwise
         */
        boolean evaluate(Facts facts);
    
        /**
         * Rule actions abstraction : this method encapsulates the rule's actions.
         * @throws Exception thrown if an exception occurs during actions performing
         */
        void execute(Facts facts) throws Exception;
    
    }

####方式二：使用@Rule注解修饰POJO
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
##使用
