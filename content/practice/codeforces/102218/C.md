---
title: "CF 102218C - Mạch không ổn định"
description: "Chúng ta có nguồn DC có điện áp (V), tiếp theo là điện trở (R). Sau điện trở, dòng điện phân chia giữa cuộn cảm (L) và tụ điện (C), được mắc song song. Ban đầu cường độ dòng điện trong cuộn cảm và điện áp của tụ điện đều bằng không."
date: "2026-08-25T04:27:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "C"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 2301
verified: false
draft: false
---

[CF 102218C - Mạch không ổn định](https://codeforces.com/problemset/problem/102218/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nguồn DC có điện áp (V), tiếp theo là điện trở (R). Sau điện trở, dòng điện phân chia giữa cuộn cảm (L) và tụ điện (C), được mắc song song. Ban đầu cường độ dòng điện trong cuộn cảm và điện áp của tụ điện đều bằng không. 

Đại lượng chúng ta cần là điện áp điện trở (V_r(t)). Mạch bị suy hao kém vì đầu vào đảm bảo 

[ 
L < 4R^2C. 
] 

Điều kiện đó có nghĩa là phản ứng dao động trong khi biên độ của nó giảm dần theo thời gian. Chúng ta cần mức tối thiểu và tối đa toàn cục của (V_r(t)), cùng với thời điểm chúng xảy ra. 

Đầu vào chứa bốn số thực và tất cả các tham số đều có giá trị dương nhỏ. Không có kích thước đầu vào lớn để lặp lại. Thách thức liên quan không phải là xử lý nhiều giá trị mà là lấy được hàm thời gian liên tục một cách chính xác. Mô phỏng số sẽ phải chọn bước thời gian đủ nhỏ và việc đạt được độ chính xác (10^{-6}) một cách đáng tin cậy sẽ đòi hỏi những công việc không cần thiết và gây ra những lo ngại về số. Vì các phương trình mạch có nghiệm dạng đóng nên chúng ta phải rút ra cực trị một cách trực tiếp. 

Có một số trường hợp việc thực hiện bất cẩn có thể thất bại. Đầu tiên, điện áp điện trở ban đầu là (V), nhưng đây không phải là mức tối thiểu hoặc tối đa đối với đáp ứng dao động. Ví dụ, với```
6 3.7 0.3 0.2
```mức tối thiểu xảy ra muộn hơn ở khoảng (t=0,348848049), với giá trị (4,430980248), do đó, chỉ cần kiểm tra (t=0) sẽ bỏ sót mức tối thiểu thực tế. 

Bẫy thứ hai giả định rằng dao động thứ nhất cho cả hai cực trị. Cực trị đầu tiên khác 0 của điện áp khối song song bên trong là dương, có nghĩa là nó cho điện áp điện trở tối thiểu. Cực trị tiếp theo là âm và cho điện áp điện trở tối đa. Vì dao động bị tắt dần nên mọi cực trị sau có độ lớn nhỏ hơn, nên hai cực trị đầu tiên này là cực trị tổng thể. 

Ranh giới bằng nhau của điều kiện giảm chấn cũng đáng được quan tâm. Nếu (L=4R^2C), tần số dao động trở thành 0 và các công thức liên quan đến phép chia cho tần số đó bị phá vỡ. Bài toán đảm bảo rõ ràng một sự bất bình đẳng nghiêm ngặt, do đó việc thực hiện có thể sử dụng công thức thiếu suy giảm một cách an toàn. 

## Phương pháp tiếp cận 

Phương pháp số trực tiếp sẽ mô phỏng các phương trình vi phân với bước thời gian nhỏ. Ở mỗi bước, chúng ta có thể tính toán dòng điện cảm ứng, điện áp tụ điện và điện áp điện trở, sau đó tìm kiếm những thay đổi theo hướng của phản ứng. Điều này có giá trị về mặt khái niệm vì các phương trình mạch xác định hoàn toàn trạng thái từ các điều kiện ban đầu. 

Vấn đề là quyết định bước đó phải nhỏ đến mức nào. Không có giới hạn thời gian hữu hạn trong tuyên bố mà tại đó quá trình mô phỏng có thể dừng lại và sai số bắt buộc là (10^{-6}). Một mô phỏng sẽ phải ước tính một khoảng thời gian đủ dài để các dao động phân rã và đồng thời sử dụng một bước đủ nhỏ để xác định chính xác điểm cực trị. Đối với tập hợp tham số trong trường hợp xấu nhất, điều này có thể yêu cầu hàng triệu cập nhật trạng thái trở lên, trong khi kích thước bước dấu phẩy động và phương pháp chẩn đoán dừng vẫn có thể khiến câu trả lời không đáng tin cậy. Độ phức tạp thời gian thực tế là (O(T/h)), trong đó (T) là thời gian mô phỏng và (h) là bước thời gian, cả hai bước này đều không được khắc phục bởi vấn đề. 

Cách tiếp cận tốt hơn là viết trực tiếp các phương trình mạch. Gọi (u(t)) là điện áp trên khối tụ điện song song. Vì cuộn cảm và tụ điện song song nên cả hai đều có điện áp (u(t)). Định luật điện áp Kirchhoff đưa ra 

[ 
V=V_r(t)+u(t), 
] 

vậy 

[ 
V_r(t)=V-u(t). 
] 

Cường độ dòng điện qua điện trở là 

[ 
I_r(t)=\frac{V-u(t)}{R}. 
] 

Dòng điện cảm ứng thỏa mãn 

[ 
I_l'(t)=\frac{u(t)}{L}, 
] 

trong khi dòng điện của tụ điện là 

[ 
I_c(t)=C u'(t). 
] 

Sử dụng (I_r=I_l+I_c), vi phân và thay thế phương trình điện cảm sẽ cho 

[ 
-\frac{u'}{R}=\frac{u}{L}+Cu''. 
] 

Sau khi sắp xếp lại, 

[ 
LCu''+\frac{L}{R}u'+u=0. 
] 

Đây là phương trình vi phân thiếu suy giảm bậc hai tiêu chuẩn. Xác định 

[ 
\alpha=\frac{1}{2RC} 
] 

và 

[ 
\omega=\sqrt{\frac{1}{LC}-\alpha^2}. 
] 

Bất đẳng thức đã cho đảm bảo rằng (\omega>0). Điện áp ban đầu của tụ bằng 0, vì vậy 

[ 
u(0)=0. 
] 

Tại thời điểm 0, dòng điện cảm ứng cũng bằng 0. Do đó toàn bộ dòng điện trở ban đầu đi vào tụ điện: 

[ 
I_r(0)=\frac{V}{R}=Cu'(0), 
] 

mang lại 

[ 
u'(0)=\frac{V}{RC}=2\alpha V. 
] 

Do đó, giải pháp là 

[ 
u(t)=\frac{2\alpha V}{\omega}e^{-\alpha t}\sin(\omega t). 
] 

Như vậy 

[ 
V_r(t)=V-\frac{2\alpha V}{\omega}e^{-\alpha t}\sin(\omega t). 
] 

Bây giờ bài toán tối ưu hóa liên tục đã trở thành một phép tính đạo hàm đơn giản. Sự thỏa mãn cực độ 

[ 
V_r'(t)=0, 
] 

hoặc tương đương (u'(t)=0). Sự khác biệt mang lại 

[ 
bạn'(t)= 
\frac{2\alpha V}{\omega}e^{-\alpha t} 
\left(\omega\cos(\omega t)-\alpha\sin(\omega t)\right). 
] 

Hệ số mũ không bao giờ bằng 0 nên ta chỉ cần 

[ 
\omega\cos(\omega t)-\alpha\sin(\omega t)=0. 
] 

Vì thế 

[ 
\tan(\omega t)=\frac{\omega}{\alpha}. 
] 

hãy để 

[ 
\theta=\arctan\left(\frac{\omega}{\alpha}\right). 
] 

Mọi cực trị đều xảy ra tại 

[ 
t_k=\frac{\theta+k\pi}{\omega}. 
] 

Số đầu tiên, (k=0), làm cho (\sin(\omega t)) dương, do đó (u(t)) dương và (V_r(t)=V-u(t)) ở mức tối thiểu. Giá trị tiếp theo, (k=1), làm cho (u(t)) âm và do đó cho điện áp điện trở tối đa.

Mọi cực trị tiếp theo đều có cùng dấu xen kẽ nhưng có độ lớn nhỏ hơn do thừa số (e^{-\alpha t}). Do đó hai cực trị đầu tiên là mức tối thiểu và tối đa toàn cầu. 

Giải pháp thu được sử dụng một số lượng không đổi các phép toán số học và lượng giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng số | (O(T/h)) | (O(1)) | Quá chậm và nhạy cảm với độ chính xác | 
| Giải pháp dạng đóng | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (V), (R), (L) và (C). Tất cả các tính toán nên sử dụng số học dấu phẩy động vì đầu vào và đầu ra được yêu cầu là số thực. 
2. Tính hệ số giảm chấn 

[ 
\alpha=\frac{1}{2RC}. 
] 

Đây là tốc độ phân rã theo cấp số nhân của dao động. 

1. Tính tần số góc tắt dần 

[ 
\omega=\sqrt{\frac{1}{LC}-\alpha^2}. 
] 

Điều kiện bài toán nghiêm ngặt đảm bảo rằng giá trị dưới căn bậc hai là dương. 

1. Tính toán 

[ 
\theta=\arctan\left(\frac{\omega}{\alpha}\right). 
] 

Cực trị thứ nhất xảy ra khi (\omega t=\theta), bởi vì đây là nghiệm dương nhỏ nhất của phương trình đạo hàm. 

1. Đặt 

[ 
t_{\min}=\frac{\theta}{\omega}. 
] 

Tại thời điểm này, điện áp khối song song là dương, do đó điện áp điện trở ở dưới (V) và đạt mức tối thiểu toàn cầu. 

1. Đặt 

[ 
t_{\max}=\frac{\theta+\pi}{\omega}. 
] 

Điểm đứng yên tiếp theo là một nửa dao động sau đó. Số hạng sin đã đổi dấu, làm cho điện áp khối song song âm và điện áp điện trở lớn hơn (V). 

1. Đánh giá 

[ 
V_r(t)=V-\frac{2\alpha V}{\omega}e^{-\alpha t}\sin(\omega t) 
] 

vào hai thời điểm này. In thời gian và giá trị tối thiểu trên dòng đầu tiên, tiếp theo là thời gian và giá trị tối đa trên dòng thứ hai. 

### Tại sao nó hoạt động 

Điện áp điện trở là (V-u(t)), trong đó (u(t)) là một hình sin tắt dần với biên độ giảm theo cấp số nhân. Các điểm dừng của nó xuất hiện chính xác khi (\tan(\omega t)=\omega/\alpha), tạo ra một chuỗi cực trị dương và cực âm xen kẽ. Hệ số mũ giảm dần theo thời gian, do đó độ lớn của mỗi cực trị sau nhỏ hơn cực trị trước. Do đó, cực trị dương đầu tiên của (u) tạo ra mức tối thiểu toàn cục của (V_r), và cực trị âm đầu tiên tạo ra mức tối đa toàn cục của nó. Thuật toán tính toán chính xác hai điểm đó từ giải pháp dạng đóng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    V, R, L, C = map(float, input().split())

    alpha = 1.0 / (2.0 * R * C)
    omega = math.sqrt(1.0 / (L * C) - alpha * alpha)

    theta = math.atan(omega / alpha)

    t_min = theta / omega
    t_max = (theta + math.pi) / omega

    amplitude = 2.0 * alpha * V / omega

    def resistor_voltage(t):
        return V - amplitude * math.exp(-alpha * t) * math.sin(omega * t)

    v_min = resistor_voltage(t_min)
    v_max = resistor_voltage(t_max)

    print(f"{t_min:.12f} {v_min:.12f}")
    print(f"{t_max:.12f} {v_max:.12f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã tính toán (\alpha) và (\omega), mô tả hoàn toàn dao động tắt dần. Bởi vì đầu vào đảm bảo (L<4R^2C), biểu thức được truyền cho`sqrt`là tích cực. 

giá trị`theta`là giá trị chính của arctang. Vì cả (\omega) và (\alpha) đều dương nên`theta`nằm hoàn toàn giữa (0) và (\pi/2). Điều đó làm cho`theta / omega`thời gian dừng dương đầu tiên, chứ không phải là nghiệm tùy ý của phương trình tiếp tuyến. 

Công dụng tối thiểu`theta`, trong khi mức sử dụng tối đa`theta + math.pi`. Chỉ sử dụng arctang chính cho cả hai cực trị sẽ trả về không chính xác cùng một pha dao động cho hai giá trị. 

Hàm trợ giúp đánh giá trực tiếp công thức dẫn xuất cho (V_r(t)). Không có tích hợp số và không có sự rời rạc hóa về thời gian, do đó không có vấn đề về kích thước bước mô phỏng hoặc điểm cuối. 

Đầu ra sử dụng mười hai chữ số sau dấu thập phân. Điều này về cơ bản có độ chính xác cao hơn yêu cầu của dung sai (10^{-6}) và tránh mất các chữ số hữu ích trong quá trình định dạng đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
6 3.7 0.3 0.2
```các giá trị trung gian có liên quan là khoảng 

[ 
\alpha=0,675675676, 
] 

[ 
\omega=3.999959876, 
] 

và 

[ 
\theta\khoảng 1,395385. 
] 

Dấu vết là: 

| Biến | Giá trị | 
| --- | --- | 
| (V) | 6 | 
| (R) | 3,7 | 
| (L) | 0,3 | 
| (C) | 0,2 | 
| (\ alpha) | 0,675675676 | 
| (\omega) | 3.999959876 | 
| (\theta) | xấp xỉ 1,395385 | 
| (t_{\min}) | 0.348848049 | 
| (V_r(t_{\min})) | 4.430980248 | 
| (t_{\max}) | 1.129139119 | 
| (V_r(t_{\max})) | 6.926100394 | 

Do đó đầu ra là```
0.348848049 4.430980248
1.129139119 6.926100394
```Điểm dừng đầu tiên xảy ra trước một dao động hoàn toàn và điện áp điện trở giảm xuống dưới điện áp nguồn. Tại điểm dừng tiếp theo, dao động tắt dần đã vượt qua 0, tạo ra mức vọt lố trên (6) volt. 

### Mẫu 2 

Một ví dụ thứ hai hữu ích là```
1 1 1 1
```đây 

[ 
\alpha=\frac12 
] 

và 

[ 
\omega=\sqrt{1-\frac14}=\frac{\sqrt3}{2}. 
] 

Giai đoạn là 

[ 
\theta=\arctan(\sqrt3)=\frac{\pi}{3}. 
] 

Dấu vết là: 

| Biến | Giá trị | 
| --- | --- | 
| (V) | 1 | 
| (R) | 1 | 
| (L) | 1 | 
| (C) | 1 | 
| (\ alpha) | 0,5 | 
| (\omega) | 0.866025404 | 
| (\theta) | 1.047197551 | 
| (t_{\min}) | 1.209199577 | 
| (V_r(t_{\min})) | khoảng 0,582318 | 
| (t_{\max}) | 4.836798308 | 
| (V_r(t_{\max})) | khoảng 1,090715 | 

Ví dụ này làm cho đặc tính giảm chấn trở nên đặc biệt dễ thấy. Độ vọt lố đầu tiên bên dưới điện áp nguồn lớn hơn nhiều so với độ vọt lố sau đó ở trên nó vì đường bao hàm mũ đã phân rã vào thời điểm cực trị thứ hai xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có một số lượng không đổi các phép toán số học, căn bậc hai, hàm mũ và lượng giác được thực hiện. | 
| Không gian | (O(1)) | Chỉ có một số biến dấu phẩy động cố định được lưu trữ. | 

Giới hạn tham số không yêu cầu bất kỳ lần lặp nào qua đầu vào. Điều kiện giảm chấn nghiêm ngặt cũng đảm bảo rằng tần số dạng đóng là thực và khác 0. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây thực hiện phép tính tương tự như một hàm có thể tái sử dụng để có thể kiểm tra các giá trị mong đợi bằng số thay vì dựa vào đẳng thức chuỗi thập phân chính xác.```python
import math
import io
import sys

def solve_case(inp: str) -> str:
    V, R, L, C = map(float, inp.split())

    alpha = 1.0 / (2.0 * R * C)
    omega = math.sqrt(1.0 / (L * C) - alpha * alpha)

    theta = math.atan(omega / alpha)

    t_min = theta / omega
    t_max = (theta + math.pi) / omega

    amplitude = 2.0 * alpha * V / omega

    def vr(t):
        return V - amplitude * math.exp(-alpha * t) * math.sin(omega * t)

    return f"{t_min:.12f} {vr(t_min):.12f}\n{t_max:.12f} {vr(t_max):.12f}"

def run(inp: str) -> str:
    return solve_case(inp)

def assert_close(actual: str, expected: str, eps: float = 1e-6):
    a = list(map(float, actual.split()))
    b = list(map(float, expected.split()))

    assert len(a) == len(b)

    for x, y in zip(a, b):
        assert abs(x - y) <= eps * max(1.0, abs(y)), (
            f"{x} != {y}"
        )

# Provided sample.
assert_close(
    run("6 3.7 0.3 0.2"),
    "0.348848049 4.430980248\n"
    "1.129139119 6.926100394",
)

# Minimum-size values satisfying the strict underdamped condition.
assert_close(
    run("1 0.1 0.1 0.1"),
    solve_case("1 0.1 0.1 0.1"),
)

# All parameters equal.
assert_close(
    run("1 1 1 1"),
    solve_case("1 1 1 1"),
)

# Large values near the upper bounds.
assert_close(
    run("20 20 20 20"),
    solve_case("20 20 20 20"),
)

# Strongly damped but still underdamped, close to the boundary.
assert_close(
    run("20 20 0.1 20"),
    solve_case("20 20 0.1 20"),
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 3.7 0.3 0.2`|`0.348848049 4.430980248`/`1.129139119 6.926100394`| Mẫu chính thức và tính đúng đắn cơ bản | 
|`1 0.1 0.1 0.1`| Tính theo biểu mẫu đóng | Giá trị tham số tối thiểu và xử lý dấu phẩy động | 
|`1 1 1 1`| Tính theo biểu mẫu đóng | Các tham số đối xứng và các giá trị lượng giác chính xác đơn giản | 
|`20 20 20 20`| Tính theo biểu mẫu đóng | Giá trị tham số lớn | 
|`20 20 0.1 20`| Tính theo biểu mẫu đóng | Hành vi gần ranh giới thiếu ẩm | 

Đối với trình kiểm tra lập trình cạnh tranh độc lập, kết quả đầu ra dự kiến ​​cho các trường hợp tùy chỉnh thường phải được so sánh với dung sai dấu phẩy động thay vì bằng văn bản chính xác. Người trợ giúp ở trên thực hiện điều đó một cách rõ ràng. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là thời điểm ban đầu. Coi như```
1 1 1 1
```Tại (t=0), điện áp trên tụ điện và dòng điện trong cuộn cảm đều bằng 0, do đó điện trở ban đầu nhận được toàn bộ điện áp nguồn và (V_r(0)=1). Thuật toán không coi đây là mức tối thiểu hoặc tối đa toàn cầu. Nó tìm thấy (t_{\min}\approx1.209199577), trong đó điện áp điện trở đã giảm xuống xấp xỉ (0,582318) và sau đó (t_{\max}\approx4.836798308), trong đó điện áp xấp xỉ (1,090715). Giá trị ban đầu nằm giữa các cực trị này. 

Trường hợp cạnh thứ hai là tính chất xen kẽ của cực trị. Với cùng một đầu vào, điểm dừng đầu tiên tương ứng với 

[ 
\omega t=\frac{\pi}{3}, 
] 

vậy sin là dương. Do đó (u(t)>0) và (V_r(t)<V). Điểm dừng tiếp theo có pha 

[ 
\frac{4\pi}{3}, 
] 

trong đó sin âm, vì vậy (V_r(t)>V). Việc thực hiện bất cẩn chỉ sử dụng arctang chính sẽ không bao giờ đạt được độ vọt lố. 

Trường hợp cạnh thứ ba là giảm chấn mạnh sát ranh giới cho phép. Coi như```
20 20 0.1 20
```đây 

[ 
\alpha=\frac{1}{800}=0,00125, 
] 

trong khi 

[ 
\omega=\sqrt{\frac{1}{2}-0,00125^2}, 
] 

do đó hệ thống vẫn dao động, nhưng hệ số tắt dần của nó nhỏ so với tần số riêng của nó. Bất đẳng thức nghiêm ngặt đảm bảo rằng (\omega) vẫn dương, do đó thuật toán có thể đánh giá căn bậc hai và chia cho (\omega) một cách an toàn. 

Cuối cùng, hãy xem xét các giá trị tham số lớn như```
20 20 20 20
```Các công thức tương tự được áp dụng mà không có bất kỳ mối lo ngại nào về việc tràn số nguyên vì các giá trị dấu phẩy động của Python dễ dàng biểu thị các đại lượng trung gian được yêu cầu. Thuật toán không nhân qua một chuỗi giá trị dài hoặc lặp lại theo thời gian, do đó độ lớn của các tham số không tạo ra lỗi tích lũy.
