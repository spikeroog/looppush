# looppush
仿小红书登录页无限轮播，轮播动画可自定义
```objc
- (void)setupUI {
    _pushView = [[UIView alloc] init];
    _pushView.backgroundColor = UIColor.clearColor;
    [self.view addSubview:_pushView];
    [_pushView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.edges.equalTo(self.view);
    }];
    
    UIImage *leftImg;
    
    if (kIsBangsScreen) {
        leftImg = [[UIImage imageNamed:@"LOGIN_PUSH"] drawImageBySize:CGSizeMake(kScreenWidth, kScreenHeight*1.8)];
    } else {
        leftImg = [[UIImage imageNamed:@"LOGIN_PUSH"] drawImageBySize:CGSizeMake(kScreenWidth, kScreenHeight*2)];
    }

    _leftImgV = [[UIImageView alloc] init];
    _leftImgV.backgroundColor = UIColor.redColor;
    _leftImgV.contentMode = UIViewContentModeScaleAspectFit;
    [_leftImgV setImage:leftImg];
    
    _leftNextImgV = [[UIImageView alloc] init];
    _leftNextImgV.backgroundColor = UIColor.redColor;
    _leftNextImgV.contentMode = UIViewContentModeScaleAspectFit;
    [_leftNextImgV setImage:leftImg];
    
    
    [_pushView addSubviews:@[_leftImgV,_leftNextImgV]];
    
    [_leftImgV mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self.pushView);
        make.left.equalTo(self.pushView);
        make.width.equalTo(@(MAIN_SCREEN_WIDTH));
        make.height.equalTo(@(leftImg.size.height));
    }];
    
    [_leftNextImgV mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self.leftImgV.mas_bottom);
        make.left.equalTo(self.leftImgV);
        make.right.equalTo(self.leftImgV);
        make.height.equalTo(@(leftImg.size.height));
    }];
    
    
    CADisplayLink *displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(loopAnimation:)];
    displayLink.frameInterval = 1.5f;//设置间隔多少帧调用一次selector方法，默认值是1，即每帧都调用一次。
    [displayLink addToRunLoop:[NSRunLoop currentRunLoop] forMode:NSDefaultRunLoopMode];
    
    
    _maskView = [[UIView alloc] init];
    _maskView.backgroundColor = kColorWithOpacity(UIColor.blackColor, 0.6f);
    [self.view addSubview:_maskView];
    [_maskView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.edges.equalTo(self.view);
    }];
    

}

#pragma mark 滚动动画

- (void)loopAnimation:(id)sener {
    ////设置左边滚动
    if (fabs(self.leftImgV.mj_y) > self.leftImgV.mj_h) {
        self.leftImgV.mj_y = self.leftNextImgV.mj_y+self.leftNextImgV.mj_h;
    }
    
    if (fabs(self.leftNextImgV.mj_y) > self.leftNextImgV.mj_h) {
        self.leftNextImgV.mj_y = self.leftImgV.mj_y+self.leftImgV.mj_h;
    }
    self.leftImgV.mj_y -= 0.75f;
    self.leftNextImgV.mj_y -= 0.75f;
}

```
