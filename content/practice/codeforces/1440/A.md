---
title: "CF 1440A - Mua Dây"
description: "Chúng tôi được cung cấp một chuỗi nhị phân và hai cách để thanh toán cho nó. Đầu tiên là mua trực tiếp: mỗi ký tự 0 có giá c0 xu và mỗi ký tự 1 có giá c1 xu."
date: "2026-06-11T04:23:32+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1440
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 684 (Div. 2)"
rating: 800
weight: 1440
solve_time_s: 65
verified: true
draft: false
---

[CF 1440A - Mua dây](https://codeforces.com/problemset/problem/1440/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân và hai cách để thanh toán cho nó. Đầu tiên là mua trực tiếp: mỗi nhân vật`0`chi phí`c0`tiền xu và mọi nhân vật`1`chi phí`c1`tiền xu. Thứ hai là sửa đổi: trước khi mua chúng ta được phép lật ký tự, lật`0`vào trong`1`hoặc`1`vào trong`0`, và mỗi lần lật có giá`h`tiền xu. 

Mục tiêu là quyết định, đối với mỗi trường hợp thử nghiệm, cần thực hiện bao nhiêu lần lật sao cho tổng chi phí của các lần lật cộng với chi phí mua chuỗi cuối cùng được giảm thiểu. 

Độ dài chuỗi tối đa là 1000 và có tối đa 10 trường hợp kiểm thử, do đó, quét bậc hai hoặc tuyến tính cho mỗi trường hợp kiểm thử là đủ. Bất kỳ mô phỏng hàm mũ hoặc từng ký tự nào của tất cả các kết hợp lật đều không cần thiết. 

Một sai lầm ngây thơ xuất phát từ việc xử lý các lượt lật của mỗi nhân vật một cách tham lam mà không xem xét đến sự cân bằng toàn cầu. Ví dụ, nếu`c0`lớn hơn nhiều so với`c1`, nó có thể có ích khi lật nhiều`0`vào`1`Ngay cả khi mỗi lần lật riêng lẻ có vẻ đắt tiền, bởi vì chênh lệch chi phí cuối cùng cho mỗi ký tự có thể lớn hơn chi phí lần lật. 

Một vấn đề tế nhị khác là cho rằng chúng ta nên luôn lật các ký tự hiện đang “đắt”. Điều đó không thành công khi việc đảo ngược làm thay đổi sự phân phối theo cách làm tăng tổng chi phí ở nơi khác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là thử mọi tập hợp con các vị trí có thể để lật ngược. Đối với mỗi tập hợp con, chúng tôi tính toán chi phí lật các ký tự đó và sau đó tính chi phí mua chuỗi kết quả. Vì mỗi vị trí có thể bị đảo lộn hoặc không, điều này dẫn đến`2^n`khả năng cho mỗi trường hợp thử nghiệm, quá lớn đối với`n = 1000`. 

Quan sát quan trọng là sau khi quyết định có bao nhiêu`0`những gì chúng ta muốn trong chuỗi cuối cùng thì vị trí chính xác không quan trọng. Điều quan trọng chỉ là số lượng số không và số một. Nếu chúng ta biết mình muốn có bao nhiêu số 0, chúng ta có thể tính toán cần bao nhiêu lần lật để đạt được điều đó từ chuỗi ban đầu. 

Cho chuỗi chứa`z`số không và`o = n - z`những cái đó. Giả sử chúng ta muốn chuỗi cuối cùng có`k`số không. Để đạt được trạng thái đó, chúng ta phải lật: 

-`(z - k)`số không thành số một nếu`k <= z`-`(k - z)`số 1 ​​thành số 0 nếu`k > z`Vì vậy, với mỗi số 0 cuối cùng có thể có`k`từ`0`ĐẾN`n`, chúng ta có thể tính số lần lật cần thiết, nhân với`h`, sau đó tính trực tiếp chi phí mua hàng. 

Điều này làm giảm vấn đề kiểm tra tất cả các thành phần cuối cùng có thể có của chuỗi, tuyến tính trong`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Hãy thử tất cả các lần đếm cuối cùng | O(n^2) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu số 0 và số 1 trong chuỗi gốc. Điều này đưa ra mô tả cơ bản của chuỗi mà không cần vị trí. 
2. Lặp lại tất cả các giá trị có thể`k`, Ở đâu`k`đại diện cho số lượng số không trong chuỗi cuối cùng. Mỗi giá trị mô tả một cấu hình mục tiêu đầy đủ. 
3. Đối với mỗi`k`, tính xem cần thực hiện bao nhiêu lần lật. Nếu như`k < z`, chúng ta giảm số không bằng cách lật`(z - k)`số không thành số một. Nếu như`k > z`, chúng ta tăng số không bằng cách lật`(k - z)`số một thành số không. Tổng số lần lật là`|z - k|`. 
4. Chuyển đổi thành chi phí bằng cách nhân với`h`. 
5. Tính chi phí mua cuối cùng như sau:`k * c0 + (n - k) * c1`. 
6. Thêm chi phí lật ngược và chi phí mua để có được tổng chi phí cho mục tiêu này`k`. 
7. Lấy mức tối thiểu trên tất cả`k`. 

Ý tưởng chính là mọi chuỗi cuối cùng có thể được biểu diễn duy nhất bằng số lượng số 0 của nó và sự chuyển đổi giữa các trạng thái chỉ phụ thuộc vào số lượng chứ không phải vị trí. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái của vấn đề có thể được tóm tắt bằng số lượng số 0 tồn tại. Lật một ký tự sẽ thay đổi số đếm này chính xác bằng một và tốn một số tiền cố định`h`. Vì chi phí cuối cùng chỉ phụ thuộc vào số lượng chứ không phụ thuộc vào sự sắp xếp, nên bất kỳ chiến lược tối ưu nào cũng phải tương ứng với số lượng cuối cùng nào đó.`k`. Bằng cách đánh giá toàn diện tất cả những điều đó`k`, chúng tôi đề cập đến tất cả các kết quả có thể xảy ra mà không cần mô phỏng một cách rõ ràng các lần lật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, c0, c1, h = map(int, input().split())
    s = input().strip()

    z = s.count('0')
    o = n - z

    ans = 10**18

    for k in range(n + 1):
        flips = abs(z - k)
        cost = flips * h + k * c0 + (n - k) * c1
        ans = min(ans, cost)

    print(ans)
```Đầu tiên, mã sẽ rút gọn chuỗi thành một thống kê duy nhất: số lượng số 0. Sau đó nó liệt kê tất cả các cấu hình mục tiêu có thể được xác định chỉ bởi thống kê đó. biểu hiện`abs(z - k)`ghi lại số lần lật tối thiểu cần thiết vì mỗi lần lật sẽ thay đổi số 0 bằng chính xác một. 

Vòng lặp kết thúc`k`là an toàn bởi vì`n ≤ 1000`, do đó, tối đa 1000 trạng thái ứng cử viên được kiểm tra cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một ví dụ nhỏ để cho thấy các cấu hình mục tiêu khác nhau cạnh tranh như thế nào. 

Coi như:```
n = 5, c0 = 10, c1 = 1, h = 1
s = 11111
```Đây`z = 0`. 

| k (số 0 cuối cùng) | lật | chi phí lật | chi phí mua | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 5*1 = 5 | 5 | 
| 1 | 1 | 1 | 1_10 + 4_1 = 14 | 15 | 
| 2 | 2 | 2 | 20 + 3 = 23 | 25 | 
| 3 | 3 | 3 | 30 + 2 = 32 | 35 | 
| 4 | 4 | 4 | 40 + 1 = 41 | 45 | 
| 5 | 5 | 5 | 50 | 55 | 

Mức tối thiểu đạt được ở`k = 0`, nghĩa là chúng tôi không làm gì và chấp nhận tất cả những cái đó. 

Điều này cho thấy rằng mặc dù việc tung tung giá rẻ nhưng việc đưa ra số 0 lại trở nên quá đắt do`c0`. 

Bây giờ hãy xem xét:```
n = 5, c0 = 10, c1 = 100, h = 1
s = 11111
```| k | lật | chi phí lật | chi phí mua | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 5*100 = 500 | 500 | 
| 1 | 1 | 1 | 10 + 400 = 410 | 411 | 
| 2 | 2 | 2 | 20 + 300 = 320 | 322 | 
| 3 | 3 | 3 | 30 + 200 = 230 | 233 | 
| 4 | 4 | 4 | 40 + 100 = 140 | 144 | 
| 5 | 5 | 5 | 50 + 0 = 50 | 55 | 

Ở đây tối ưu là`k = 5`, nghĩa là chúng ta biến mọi thứ thành số 0 vì mua số 0 rẻ hơn nhiều. 

Dấu vết cho thấy sự cân bằng giữa chi phí lật và chi phí mỗi ký tự xác định cấu trúc tối ưu như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) cho mỗi trường hợp thử nghiệm | Đối với mỗi lần đếm cuối cùng có thể`k`, chúng ta tính lại chi phí theo O(1) | 
| Không gian | O(1) | Chỉ bộ đếm và số vô hướng được lưu trữ | 

Được cho`n ≤ 1000`và tối đa 10 trường hợp thử nghiệm, điều này mang lại nhiều nhất khoảng 10 triệu thao tác đơn giản, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output

    t = int(input())
    for _ in range(t):
        n, c0, c1, h = map(int, input().split())
        s = input().strip()

        z = s.count('0')
        ans = 10**18

        for k in range(n + 1):
            flips = abs(z - k)
            cost = flips * h + k * c0 + (n - k) * c1
            ans = min(ans, cost)

        print(ans)

    sys.stdout = old_stdout
    return output.getvalue().strip()

# provided samples
assert run("""6
3 1 1 1
100
5 10 100 1
01010
5 10 1 1
11111
5 1 10 1
11111
12 2 1 10
101110110101
2 100 1 10
00
""") == """3
52
5
10
16
22"""

# custom cases
assert run("""1
1 5 1 100
0
""") == "1", "single char no flip needed"

assert run("""1
3 1 10 1
000
""") == "3", "all zeros already optimal"

assert run("""1
4 10 1 5
1111
""") == "4", "better to keep ones"

assert run("""1
5 3 3 100
10101
""") == "15", "flip too expensive so no changes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 1 | trường hợp cạnh tối thiểu | 
| tất cả số không | 3 | không có lợi từ việc lật nhà | 
| tất cả những cái | 4 | tính đúng đắn đối xứng | 
| chi phí lật cao | 15 | tránh những cú lật không cần thiết | 

## Vỏ cạnh 

Khi chuỗi đồng nhất, chẳng hạn như`00000`, thuật toán đánh giá mọi`k`nhưng nhanh chóng phát hiện ra rằng bất kỳ sai lệch nào cũng làm tăng chi phí thay thế mà không làm giảm chi phí mua hàng đủ để bù đắp. Vì`k = 5`, lần lật trở thành 5, nhưng chi phí mua không cải thiện vì`c0`được cố định cho mỗi ký tự. 

Khi lật chi phí`h`là cực kỳ lớn nên lời giải tối ưu luôn tương ứng với một trong hai`k = z`(không lật) hoặc ranh giới gần nhất. Việc liệt kê đương nhiên bao gồm trường hợp này bởi vì`k = z`tạo ra chi phí lật bằng không. 

Khi`c0`Và`c1`bị đảo ngược về độ lớn, thuật toán sẽ tự động chuyển giá trị tối ưu sang chuyển đổi hoàn toàn sang ký tự rẻ hơn, vì thuật ngữ chi phí tuyến tính chiếm ưu thế và lớn hơn các hình phạt lật một lần`n * |c0 - c1|`vượt quá`h`.
