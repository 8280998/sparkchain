# sparkchain
sparkchain刷邮箱验证积分

使用我的邀请注册 https://sparkchain.ai/register/?r=47286738

进入https://sparkchain.ai/tasks

按F12进入开发模式，点击console,输入以下代码

function simulateClick(element) {
    let event = new MouseEvent('click', { bubbles: true, cancelable: true, view: window });
    element.dispatchEvent(event);
}

setInterval(function() {
    console.log('Running check at:', new Date().toLocaleTimeString()); // 每次循环都输出，确认脚本活跃

    let cards = document.querySelectorAll('div.border.bg-card.text-card-foreground.shadow-sm');
    console.log('Found cards:', cards.length); // 检查找到多少卡片

    for (let card of cards) {
        let rewardSpan = card.querySelector('span.font-semibold.text-primary.text-lg');
        if (rewardSpan) {
            let rewardText = rewardSpan.textContent.trim();
            console.log('Reward text found:', rewardText); // 输出实际文本，便于调试
            if (rewardText.includes('10,000')) { // 更灵活匹配，忽略符号和 'Point'
                let button = card.querySelector('button.btn-green');
                if (button) {
                    console.log('Button text:', button.textContent.trim(), 'Disabled:', button.disabled);
                    if (button.textContent.trim() === 'Claim' && !button.disabled) {
                        console.log('Attempting to click Claim button at:', new Date().toLocaleTimeString());
                        simulateClick(button);
                        console.log('Clicked Claim button, checking response...');
                        setTimeout(() => {
                            if (!button || button.textContent.trim() !== 'Claim') {
                                console.log('Button state changed, likely clicked successfully! Check app/dashboard for points update.');
                            } else {
                                console.log('Button still exists, click may have failed. Verify email or check Network errors.');
                            }
                        }, 2000); // 2秒延迟检查
                    }
                }
                break;
            }
        }
    }
}, Math.random() * 2000 + 1000); // 随机间隔
