import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { Label } from "@/components/ui/label";

export default function BettingPlatform() {
  const [formData, setFormData] = useState({ hero: "", amount: "", matchUrl: "" });
  const [submitted, setSubmitted] = useState(false);
  const [authMode, setAuthMode] = useState("login");
  const [authEmail, setAuthEmail] = useState("");
  const [authCode, setAuthCode] = useState("");
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = () => {
    setSubmitted(true);
    // 提交下注数据到后台，包括英雄、金额和比赛链接
  };

  const handleSendCode = () => {
    alert(`验证码已发送至 ${authEmail}`);
  };

  const handleAuthSubmit = () => {
    if (authCode === "123456") {
      setIsAuthenticated(true);
    } else {
      alert("验证码错误，请重试。");
    }
  };

  if (!isAuthenticated) {
    return (
      <div className="p-6 max-w-md mx-auto">
        <h1 className="text-xl font-bold mb-4">{authMode === "login" ? "登录账号" : "注册账号"}</h1>
        <div className="space-y-4">
          <div>
            <Label>邮箱地址</Label>
            <Input
              type="email"
              value={authEmail}
              onChange={(e) => setAuthEmail(e.target.value)}
              placeholder="请输入邮箱"
            />
          </div>
          <div>
            <Button onClick={handleSendCode}>发送验证码</Button>
          </div>
          <div>
            <Label>验证码</Label>
            <Input
              value={authCode}
              onChange={(e) => setAuthCode(e.target.value)}
              placeholder="输入邮箱收到的验证码"
            />
          </div>
          <Button className="w-full" onClick={handleAuthSubmit}>
            {authMode === "login" ? "登录" : "注册并登录"}
          </Button>
          <p
            className="text-sm text-blue-600 cursor-pointer"
            onClick={() => setAuthMode(authMode === "login" ? "register" : "login")}
          >
            {authMode === "login" ? "没有账号？点此注册" : "已有账号？点此登录"}
          </p>
        </div>
      </div>
    );
  }

  return (
    <div className="p-6 max-w-4xl mx-auto">
      <h1 className="text-2xl font-bold mb-4">王者荣耀评分斗牛下注平台</h1>
      <p className="mb-4 text-gray-700">
        ⚠️ 请通过微信群内分享的游戏直播链接进入比赛，比赛开始前完成下注。平台可自动识别直播链接中的可选英雄，比赛结束后系统后台将自动抓取战绩截图并分析英雄评分进行比牌。
      </p>

      <Tabs defaultValue="bet" className="w-full">
        <TabsList>
          <TabsTrigger value="bet">下注</TabsTrigger>
          <TabsTrigger value="upload">上传战绩</TabsTrigger>
          <TabsTrigger value="admin">后台管理</TabsTrigger>
          <TabsTrigger value="stream">直播观看</TabsTrigger>
          <TabsTrigger value="usdt">充值说明</TabsTrigger>
        </TabsList>

        <TabsContent value="bet">
          <Card className="mt-4">
            <CardContent className="space-y-4">
              <div>
                <Label>比赛链接</Label>
                <Input
                  name="matchUrl"
                  placeholder="粘贴微信群分享的直播链接"
                  value={formData.matchUrl}
                  onChange={handleChange}
                />
              </div>
              <div>
                <Label>下注英雄名</Label>
                <Input
                  name="hero"
                  placeholder="如：鲁班、李白"
                  value={formData.hero}
                  onChange={handleChange}
                />
              </div>
              <div>
                <Label>下注金额（积分，充值方式见说明）</Label>
                <Input
                  name="amount"
                  type="number"
                  placeholder="最低下注100积分"
                  value={formData.amount}
                  onChange={handleChange}
                />
              </div>
              <Button className="w-full" onClick={handleSubmit}>
                提交下注
              </Button>
              {submitted && (
                <div className="text-green-600">
                  ✅ 下注成功！系统将根据比赛结束后的战绩自动比牌。
                </div>
              )}
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="upload">
          <Card className="mt-4">
            <CardContent className="space-y-4">
              <Label>上传比赛战绩截图（备用手动上传）</Label>
              <Input type="file" />
              <Button className="w-full">上传</Button>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="admin">
          <Card className="mt-4">
            <CardContent className="space-y-4">
              <p className="text-sm text-muted">
                后台管理面板：展示所有下注记录、彩蛋中奖信息、用户数据、比赛链接同步结果。
              </p>
              <Button variant="outline">同步抓取最新战绩</Button>
              <Button variant="outline">导出下注记录表</Button>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="stream">
          <Card className="mt-4">
            <CardContent className="space-y-4">
              <Label>实时比赛直播</Label>
              <iframe
                className="w-full h-[480px]"
                src="https://example.com/stream?hideDanmu=1&hideGifts=1"
                title="直播视频"
                frameBorder="0"
                allowFullScreen
              ></iframe>
              <p className="text-sm text-muted">已自动屏蔽弹幕与礼物特效，提升观赛体验。</p>
            </CardContent>
          </Card>
        </TabsContent>

        <TabsContent value="usdt">
          <Card className="mt-4">
            <CardContent className="space-y-2">
              <h2 className="text-lg font-semibold">💳 积分充值与提现说明</h2>
              <ul className="text-sm list-disc pl-5 space-y-1">
                <li>当前仅支持通过 <strong>USDT (TRC20)</strong> 方式充值获取积分。</li>
                <li>积分用于下注，不可转赠。</li>
                <li>兑奖与提现仅支持 <strong>USDT 收款</strong>，请确保填写正确钱包地址。</li>
                <li>充值到账时间一般为 1~5 分钟，若延迟请联系在线客服。</li>
              </ul>
            </CardContent>
          </Card>
        </TabsContent>
      </Tabs>
    </div>
  );
}
