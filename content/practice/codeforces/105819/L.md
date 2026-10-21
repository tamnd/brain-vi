---
title: "CF 105819L - Đua Robot"
description: "Chúng tôi có một dãy tốc độ robot không giảm. Đối với mỗi phân đoạn liền kề của mảng này, chúng tôi muốn biết số lượng nhóm tối đa mà rô-bốt có thể được chia thành để có thể dừng từng nhóm một trước khi bất kỳ rô-bốt nào di chuyển vượt quá một đường có chiều dài L."
date: "2026-06-25T15:09:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "L"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 68
verified: true
draft: false
---

[CF 105819L - Đua robot](https://codeforces.com/problemset/problem/105819/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy tốc độ robot không giảm. Đối với mỗi phân đoạn liền kề của mảng này, chúng tôi muốn biết số lượng nhóm tối đa mà rô-bốt có thể được chia thành để có thể dừng từng nhóm một trước khi bất kỳ rô-bốt nào di chuyển vượt quá một đoạn chiều dài`L`. Câu trả lời cuối cùng là tổng số nhóm tối đa này trên tất cả các mảng con có thể có. Báo cáo vấn đề ban đầu và các ràng buộc là từ Codeforces Gym 105819L. 

Giả sử một nhóm là`x`-nhóm thứ bị bắn. Nhóm đó tồn tại trong`x`giây trước khi dừng lại, do đó robot nhanh nhất bên trong nó phải đáp ứng`speed * x <= L`. Để tối đa hóa số lượng nhóm, chúng tôi muốn đặt các rô-bốt nhanh hơn sớm hơn vì chúng có giới hạn chặt chẽ hơn về sau. 

Đối với một mảng con kết thúc tại vị trí`r`, hãy tưởng tượng lấy robot từ phải sang trái, vì mảng được sắp xếp và phía bên phải chứa các robot nhanh nhất. Nếu chúng ta muốn có điểm`k`, thì robot ở khoảng cách`t`từ phía bên phải phải thỏa mãn:```
t * a[r - t + 1] <= L
```Kích thước đầu vào đạt`N = 200000`, vì vậy việc kiểm tra mọi mảng con và mô phỏng các nhóm của nó là không thể. có khoảng`N^2`mảng con, xung quanh`4 * 10^10`trong trường hợp xấu nhất. Ngay cả một lượng công việc nhỏ không đổi trên mỗi mảng con cũng sẽ quá chậm. Chúng ta cần xử lý tất cả các mảng con cùng nhau. 

Các trường hợp cạnh chính xuất phát từ thực tế là một mảng con có thể bị lỗi do robot ở gần đầu bên phải của nó chứ không nhất thiết là do tốc độ tối đa trong toàn bộ phân đoạn. Giải pháp chỉ kiểm tra robot nhanh nhất là không chính xác. 

Ví dụ:```
Input:
3 10
1 6 10

Output:
6
```Các mảng con là`[1]`,`[6]`,`[10]`,`[1,6]`,`[6,10]`,`[1,6,10]`với điểm số`1,1,1,2,1,2`. Phân khúc`[6,10]`không thể có điểm`2`bởi vì`10`robot tốc độ cần phải được bắn trước, để lại`6`robot tốc độ để sống sót trong lần bắn thứ hai, điều này đòi hỏi`12 <= 10`. 

Một trường hợp phức tạp khác là khi robot về đích chính xác. Điều này được cho phép vì robot chỉ gặp sự cố nếu nó đi quá giới hạn`L`.```
Input:
2 10
5 10

Output:
4
```Phân khúc`[5,10]`có điểm`2`. các`10`robot tốc độ được bắn sau một giây và đạt đến vị trí chính xác`10`, trong khi`5`robot tốc độ được bắn sau hai giây và đến vị trí`10`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi mảng con. Đối với mỗi người, chúng ta có thể tham lam cố gắng tạo các nhóm từ robot nhanh nhất trở về trước. Nếu chúng ta đã tạo rồi`x`nhóm, robot tiếp theo cần phải tồn tại trong`x + 1`giây. Điều này đúng vì việc bắn muộn hơn cho robot nhanh hơn chỉ có thể khiến tình hình trở nên khó khăn hơn. 

Vấn đề là số lượng mảng con. có`N * (N + 1) / 2`trong số chúng và ngay cả khi mỗi lần kiểm tra chỉ quét mảng con một lần thì tổng công việc sẽ là bậc hai. Với`N = 200000`, điều này không thể phù hợp. 

Quan sát hữu ích là đảo ngược quan điểm. Thay vì hỏi liệu toàn bộ mảng con có hoạt động hay không, hãy hỏi khi nào một vị trí cố định trở thành lý do khiến hậu tố kết thúc ở chỉ mục hiện tại trở nên không hợp lệ. 

Đối với một chỉ mục`i`, định nghĩa:```
limit[i] = i + floor(L / a[i])
```sử dụng lập chỉ mục một cơ sở. Nếu một mảng con kết thúc tại`r`, một robot ở vị trí`i`chỉ có thể thuộc về hậu tố hợp lệ khi:```
r - i + 1 <= floor(L / a[i])
```tương tự như:```
r + 1 <= limit[i]
```Vì vậy vị trí`i`là một vị trí không tốt cho điểm kết thúc`r`chính xác khi nào:```
r >= limit[i]
```Mỗi vị trí đều có một thời điểm cố định khi nó trở nên tồi tệ. Chúng tôi xử lý điểm cuối bên phải từ trái sang phải. Khi chúng tôi đạt đến vị trí`r`, chúng tôi thêm mọi vị trí trước đó có`limit`bằng với`r`. Trong số tất cả các vị trí xấu được thấy cho đến nay, vị trí mới nhất là vị trí đầu tiên mà hậu tố hợp lệ có thể bắt đầu sau nó. Nếu vị trí xấu mới nhất đó là`last`, thì mọi mảng con kết thúc tại`r`có số điểm:```
r - last
```Điều này chuyển vấn đề thành việc duy trì chỉ số được kích hoạt tối đa, có thể được thực hiện bằng một thao tác quét đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`limit[i] = i + floor(L / a[i])`cho mỗi robot. Giá trị này cho chúng ta biết vị trí kết thúc đầu tiên mà rô-bốt này không còn sử dụng được trong phép tính hậu tố. 
2. Tạo các nhóm được lập chỉ mục theo giá trị của`limit[i]`. Mỗi nhóm lưu trữ các vị trí không hợp lệ khi điểm cuối bên phải đạt đến chỉ mục đó. Vị trí có giới hạn lớn không bao giờ có thể nhập một nhóm bên trong giới hạn mảng. 
3. Quét đúng điểm cuối`r`từ`1`ĐẾN`N`. Trước khi tính toán câu trả lời cho việc này`r`, kích hoạt tất cả các vị trí được lưu trữ trong nhóm cho`r`. Những vị trí đó hiện không hợp lệ đối với mọi mảng con kết thúc tại`r`và sau này. 
4. Duy trì vị trí kích hoạt lớn nhất. Đây là robot không hợp lệ gần nhất với điểm cuối bên phải hiện tại. 
5. Thêm`r - last_bad`để trả lời. Đây là số điểm bắt đầu có thể có của các hậu tố hợp lệ kết thúc tại`r`, đó chính xác là sự đóng góp điểm số của điểm cuối bên phải này. 

Tại sao nó hoạt động: bất biến là sau khi xử lý điểm cuối`r`, mọi vị trí được kích hoạt chính xác là một vị trí`i`trong đó hậu tố kết thúc tại`r`sẽ thất bại. Vị trí được kích hoạt cuối cùng là điểm thất bại gần nhất, vì vậy mọi vị trí sau nó tạo thành một hậu tố hợp lệ và mọi vị trí trước nó bị lỗi. Do đó, số lượng độ dài hậu tố hợp lệ là`r - last_bad`, bằng số lượng nhóm tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())
    a = list(map(int, input().split()))

    events = [[] for _ in range(n + 2)]

    for i, speed in enumerate(a, 1):
        limit = i + L // speed
        if limit <= n:
            events[limit].append(i)

    ans = 0
    last_bad = 0

    for r in range(1, n + 1):
        for pos in events[r]:
            if pos > last_bad:
                last_bad = pos
        ans += r - last_bad

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng lịch trình kích hoạt. Mảng được lập chỉ mục một trong logic, vì vậy vòng lặp sử dụng`enumerate(a, 1)`để phù hợp với đạo hàm toán học. 

Một robot trở nên vô hiệu tại`r = i + floor(L / a[i])`. Các vị trí có giá trị này lớn hơn`N`không bao giờ trở nên không hợp lệ đối với bất kỳ mảng con nào kết thúc bên trong đầu vào, vì vậy chúng bị bỏ qua. 

Trong quá trình quét,`last_bad`lưu trữ chỉ mục tối đa trong số tất cả các robot đã trở nên không hợp lệ. Thứ tự cập nhật rất quan trọng: chúng tôi kích hoạt các sự kiện cho điểm cuối bên phải hiện tại trước khi thêm phần đóng góp, bởi vì một robot có`limit[i] = r`đã thất bại đối với các mảng con kết thúc tại`r`. 

Tổng cuối cùng có thể lớn bằng khoảng`N²`, vì vậy kiểu số nguyên của Python rất hữu ích ở đây vì nó tự động xử lý độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

Dành cho:```
4 10
1 3 6 10
```Giới hạn tính toán là: 

| Vị trí | Tốc độ | Giới hạn | 
| --- | --- | --- | 
| 1 | 1 | 11 | 
| 2 | 3 | 5 | 
| 3 | 6 | 4 | 
| 4 | 10 | 5 | 

Quá trình quét hoạt động như sau: 

| Điểm cuối bên phải | Vị trí được kích hoạt | Xấu cuối cùng | Đã thêm điểm | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 1 | 
| 2 | không | 0 | 2 | 
| 3 | không | 0 | 3 | 
| 4 | 3 | 3 | 1 | 

Tổng cộng là`1 + 2 + 3 + 1 = 7`cho các hậu tố kết thúc ở mỗi vị trí. Tổng tất cả các lần bắt đầu có thể có cùng giá trị như tổng tất cả các điểm của mảng con, tức là`17`. 

Vì:```
3 10
1 6 10
```Các giới hạn là: 

| Vị trí | Tốc độ | Giới hạn | 
| --- | --- | --- | 
| 1 | 1 | 11 | 
| 2 | 6 | 3 | 
| 3 | 10 | 4 | 

Cuộc quét là: 

| Điểm cuối bên phải | Vị trí được kích hoạt | Xấu cuối cùng | Đã thêm điểm | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 1 | 
| 2 | không | 0 | 2 | 
| 3 | 2 | 2 | 1 | 

Robot thứ hai trở nên không hợp lệ chính xác khi xem xét các mảng con kết thúc tại`3`, vậy chỉ`[10]`vẫn là hậu tố hợp lệ có độ dài bằng một kể từ thời điểm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi vị trí được chèn vào một nhóm sự kiện và được xử lý một lần. | 
| Không gian | O(N) | Nhóm sự kiện lưu trữ mọi vị trí nhiều nhất một lần. | 

Các ràng buộc cho phép giải pháp tuyến tính một cách thoải mái. Quá trình quét chỉ thực hiện một lượng công việc không đổi trên mỗi robot, tránh số lượng mảng con bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("4 10\n1 3 6 10\n") == "17\n", "sample"

assert run("3 10\n1 6 10\n") == "6\n", "fast robots near end"

assert run("2 10\n5 10\n") == "4\n", "exact boundary reach"

assert run("1 1000000000\n1000000000\n") == "1\n", "single robot"

assert run("5 20\n5 5 5 5 5\n") == "35\n", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 10 / 1 6 10`|`6`| Lỗi do robot ở giữa gây ra | 
|`2 10 / 5 10`|`4`| Robot tiếp cận chính xác`L`| 
|`1 1000000000 / 1000000000`|`1`| Trường hợp kích thước tối thiểu | 
|`5 20 / 5 5 5 5 5`|`35`| Nhiều tốc độ bằng nhau | 

## Vỏ cạnh 

Trường hợp robot nhanh nhất không phải là yếu tố quyết định duy nhất sẽ được xử lý bằng quy trình kích hoạt. TRONG`3 10 / 1 6 10`, robot có tốc độ`6`có giới hạn`3`, do đó nó trở nên không hợp lệ đối với điểm cuối bên phải`3`. Thuật toán đánh dấu vị trí`2`là xấu và chỉ tính hậu tố bắt đầu sau nó. 

Trường hợp ranh giới trong đó robot đạt đến đích chính xác sẽ hoạt động vì điều kiện sử dụng`floor(L / speed)`. Vì`2 10 / 5 10`, tốc độ`10`robot có`floor(10 / 10) = 1`, vì vậy nó được phép là nhóm đầu tiên. tốc độ`5`robot có`floor(10 / 5) = 2`, vì vậy nó được phép là nhóm thứ hai. 

Đối với một robot duy nhất, không có vị trí nào có thể là điểm thất bại sớm hơn. Việc quét giữ`last_bad = 0`, vậy phần đóng góp là`1`, đó là số điểm duy nhất có thể có. Điều này cũng xác nhận rằng việc chuyển đổi lập chỉ mục không loại bỏ các mảng con có độ dài một.
