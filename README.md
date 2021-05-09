不可变对象就是这样一种在创建之后就不再变更的对象，这种特性使得它们天生支持线程安全
如何创建不可变对象
Collections.unmodifiableList(List<? extends T> list)
　　另外，在Google的Guava包中也提供了一系列方法来创建不可变集合，如：

1
ImmutableList.copyOf(list)
Lombok通过@Value注释
允许使用字段级@Set进行完全可变，但在需要不可变性时提供@Value

方法内参数校验使用import org.springframework.util.Assert;

  public void drive(int speed) {
        Assert.isTrue(speed > 0, "speed must be positive");
        this.state = "drive";
        // ...
        if (!(speed > 0)) {
            throw new IllegalArgumentException("speed must be >0");
        }
       
       public void сhangeOil(String oil) {
        Assert.notNull(oil, "oil mustn't be null");
        // ...
    }
      Assert.hasText(key, "key must not be null and must contain at least one non-whitespace  character");
        
    }

自定义异常例子
   public String getActor(int index) throws ActorNotFoundException {
        if (index >= actors.size()) {
            throw new ActorNotFoundException("Actor Not Found in Repsoitory");
        }
        return actors.get(index);
    }




stream 流写法 以及 InitializingBean 初始化时候执行业务逻辑。	
@Component
public class DataSetupBean implements InitializingBean {

    @Autowired
    private FooRepository repo;

    //

    @Override
    public void afterPropertiesSet() throws Exception {
        IntStream.range(1, 20)
            .forEach(i -> repo.save(new Foo(randomAlphabetic(8))));
    }

}

并发时使用原子类型
  private final AtomicInteger count = new AtomicInteger(0);

    @Scheduled(fixedDelay = 5)
    public void scheduled() {
        this.count.incrementAndGet();
    }

分页例子
  @GetMapping(params = { "page", "size" })
    @ResponseBody
    @Validated
    public List<Foo> findPaginated(@RequestParam("page") @Min(0) final int page, @Max(100) @RequestParam("size") final int size) {
        return repo.findAll(PageRequest.of(page, size))
            .getContent();
    }

   逻辑判断，用异常返回给前端提示
    @GetMapping("/delete/{id}")
    public String deleteUser(@PathVariable("id") long id, Model model) {
        User user = userRepository.findById(id).orElseThrow(() -> new IllegalArgumentException("Invalid user Id:" + id));
        userRepository.delete(user);
        
        return "index";
    }
