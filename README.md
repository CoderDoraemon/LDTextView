# LDTextView
继承于UITextView的自定义TextView, 带placeholder和可限制最大输入字符数

#import "ViewController.h"
#import "LDTextView.h"

@interface ViewController ()

@property (weak, nonatomic) IBOutlet LDTextView *nibTextView;

@property (weak, nonatomic) IBOutlet UILabel *tipLabel;


/** textView */
@property (nonatomic, strong) LDTextView *textView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    
    [self setup];
}

- (void)viewWillLayoutSubviews {
    [super viewWillLayoutSubviews];
    
    self.textView.frame = CGRectMake(self.nibTextView.frame.origin.x, CGRectGetMaxY(self.tipLabel.frame) + 20, self.nibTextView.bounds.size.width, self.nibTextView.bounds.size.height);
}

- (void)setup {
    
    [self.view addSubview:self.textView];
    
    __weak typeof(self.tipLabel) weakTipLabel = self.tipLabel;
    // 添加输入改变Block回调.
    [self.nibTextView addTextDidChangeHandler:^(LDTextView *textView) {
        (textView.text.length < textView.maxLength) ? weakTipLabel.text = @"":NULL;
    }];
    // 添加到达最大限制Block回调.
    [self.nibTextView addTextLengthDidMaxHandler:^(LDTextView *textView) {
        weakTipLabel.text = [NSString stringWithFormat:@"最多限制输入%zi个字符", textView.maxLength];
    }];
    
}


- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    [self.view endEditing:YES];
}


#pragma mark - lazy
- (LDTextView *)textView {
    if (_textView == nil) {
        _textView = [LDTextView textView];
        _textView.placeholder = @"这是一个继承于UITextView的带Placeholder的自定义TextView, 可以设定限制字符长度, 以Block形式回调, 简单直观 !";
        _textView.placeholderColor = [UIColor orangeColor];
        _textView.borderWidth = 1.f;
        _textView.borderColor = UIColor.lightGrayColor;
        _textView.cornerRadius = 5.f;
    }
    return _textView;
}
