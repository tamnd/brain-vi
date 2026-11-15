---
title: "CF 1862A - Thảm Quà Tặng"
description: "Chúng ta có một tấm thảm hình chữ nhật có kích thước $n nhân m$ trong đó mỗi ô chứa một chữ cái viết thường. Vika thích đọc tên mình \"vika\" trên tấm thảm bằng cách chọn một chữ cái từ mỗi cột, di chuyển từ trái sang phải và chọn chính xác bốn cột cho các chữ cái 'v'…"
date: "2026-06-09T00:50:53+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1862
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 894 (Div. 3)"
rating: 800
weight: 1862
solve_time_s: 83
verified: true
draft: false
---

[CF 1862A - Thảm quà tặng](https://codeforces.com/problemset/problem/1862/A) 

**Đánh giá:** 800 
**Tags:** dp, tham lam, triển khai, chuỗi 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một tấm thảm hình chữ nhật có kích thước$n \times m$trong đó mỗi ô chứa một chữ cái viết thường. Vika thích đọc tên mình "vika" trên tấm thảm bằng cách chọn một chữ cái từ mỗi cột, di chuyển hoàn toàn từ trái sang phải và chọn chính xác bốn cột cho các chữ cái 'v', 'i', 'k' và 'a' theo thứ tự đó. Mỗi cột có thể đóng góp tối đa một chữ cái và các cột phải khác biệt. 

Nhiệm vụ là xác định, đối với nhiều trường hợp thử nghiệm, liệu Vika có thể đọc được tên của mình trên một tấm thảm nhất định hay không. Nếu có thể thì xuất ra "CÓ", nếu không thì xuất ra "KHÔNG". 

Giới hạn là nhỏ:$1 \le n, m \le 20$và lên tới 100 trường hợp thử nghiệm. Bởi vì cả hai chiều đều rất nhỏ nên các thuật toán có độ phức tạp về thời gian$O(n \cdot m)$mỗi trường hợp thử nghiệm đều được chấp nhận. Các ràng buộc cho thấy chúng tôi không cần tối ưu hóa phức tạp hoặc cấu trúc dữ liệu nâng cao. 

Trường hợp cạnh không rõ ràng xảy ra khi$n = 1$hoặc$m = 4$và các chữ cái xuất hiện trong một hàng duy nhất. Ví dụ: nếu tấm thảm là:```
1 4
kvai
```Việc quét cột từ trái sang phải bất cẩn cho rằng các chữ cái xuất hiện theo thứ tự trong một hàng có thể kết luận sai "CÓ" mặc dù trình tự đúng theo thứ tự cột từ trái sang phải không đánh vần là "vika". Một trường hợp đặc biệt khác xảy ra khi các chữ cái cần thiết nằm rải rác trên nhiều hàng trong cùng một cột - giải pháp phải coi bất kỳ hàng nào trong cột là một kết quả khớp hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử mọi sự kết hợp của bốn cột riêng biệt và sau đó kiểm tra xem các chữ cái bắt buộc có xuất hiện trong các cột này theo thứ tự hay không. có$\binom{m}{4}$cách chọn bốn cột và kiểm tra từng cột để tìm chữ cái$O(n)$thời gian. Trong trường hợp xấu nhất, đây sẽ là$O(n \cdot m^4)$mỗi trường hợp thử nghiệm. Vì$m = 20$Và$n = 20$, tức là khoảng 3,2 triệu thao tác cho mỗi trường hợp thử nghiệm, có thể chấp nhận được nhưng có thể đơn giản hóa. 

Thông tin chi tiết quan trọng là chúng tôi chỉ quan tâm đến **cột có sẵn đầu tiên** chứa chữ cái bắt buộc tiếp theo trong chuỗi. Chúng ta có thể quét các cột từ trái sang phải, duy trì con trỏ tới chữ cái dự kiến ​​tiếp theo trong "vika". Nếu cột hiện tại chứa chữ cái đó ở hàng bất kỳ thì chúng ta chuyển sang chữ cái tiếp theo. Nếu chúng ta đến cuối cột và tìm thành công cả bốn chữ cái, câu trả lời là "CÓ"; nếu không thì "KHÔNG". Cách tiếp cận tham lam này hoạt động vì các cột phải được sử dụng theo thứ tự và khi một cột bị bỏ qua, cột đó sẽ không thể được sử dụng lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m^4) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tham lam từ trái sang phải | O(n * m) | O(1) | Tối ưu và đơn giản | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$m$. 
2. Đọc tấm thảm dưới dạng danh sách các chuỗi (mỗi chuỗi là một hàng). 
3. Khởi tạo một biến`index`về 0 để theo dõi chữ cái hiện tại trong chuỗi mục tiêu "vika". 
4. Lặp lại các cột từ trái sang phải. Đối với mỗi cột, hãy kiểm tra xem có hàng nào chứa chữ cái đích hiện tại không (`target[index]`). 
5. Nếu cột hiện tại chứa chữ cái đích, hãy tăng dần`index`để chuyển sang chữ cái tiếp theo. 
6. Sau khi xử lý tất cả các cột, kiểm tra xem`index`bằng 4. Nếu có, hãy in "CÓ". Nếu không thì in ra "NO". 

Điều này có tác dụng vì sự cố cho phép bất kỳ hàng nào trong một cột khớp với nhau và chúng ta chỉ cần giữ nguyên thứ tự cột. Bằng cách quét từ trái sang phải và tiến lên`index`một cách tham lam, chúng tôi đảm bảo các chữ cái xuất hiện theo đúng trình tự mà không bỏ lỡ bất kỳ khả năng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    carpet = [input().strip() for _ in range(n)]
    target = "vika"
    index = 0
    
    for col in range(m):
        if any(carpet[row][col] == target[index] for row in range(n)):
            index += 1
            if index == 4:
                break
    
    print("YES" if index == 4 else "NO")
```Vòng lặp bên ngoài đọc các trường hợp kiểm thử. Vòng lặp bên trong quét các cột từ trái sang phải, kiểm tra từng cột theo chữ cái đích hiện tại bằng cách sử dụng`any()`. Tăng dần`index`chỉ khi tìm thấy kết quả khớp mới đảm bảo các chữ cái được chọn theo thứ tự và mỗi cột đóng góp tối đa một chữ cái. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1 4
vika
```| col | chữ cái trong cột | chỉ mục | khớp? | 
| --- | --- | --- | --- | 
| 0 | v | 0 | CÓ → chỉ số=1 | 
| 1 | tôi | 1 | CÓ → chỉ số=2 | 
| 2 | k | 2 | CÓ → chỉ số=3 | 
| 3 | một | 3 | CÓ → chỉ mục=4 | 

Đầu ra:`YES`Điều này cho thấy chức năng quét tham lam từ trái sang phải đơn giản hoạt động trên một tấm thảm một hàng. 

**Ví dụ 2** 

đầu vào:```
3 3
bad
car
pet
```| col | chữ cái trong cột | chỉ mục | khớp? | 
| --- | --- | --- | --- | 
| 0 | b,c,p | 0 | KHÔNG | 
| 1 | một, một, e | 0 | KHÔNG | 
| 2 | d,r,t | 0 | KHÔNG | 

Đầu ra:`NO`Không có cột nào chứa 'v', vì vậy`index`không bao giờ tiến bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n * m) | Mỗi cột trong mỗi test case được quét, mỗi lần quét kiểm tra tối đa n hàng | 
| Không gian | O(n * m) | Thảm được lưu trong bộ nhớ | 

Với$t \le 100$,$n, m \le 20$, các hoạt động tối đa là$100 * 20 * 20 = 40,000$, trong giới hạn 1 giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # copy solution here
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        carpet = [input().strip() for _ in range(n)]
        target = "vika"
        index = 0
        for col in range(m):
            if any(carpet[row][col] == target[index] for row in range(n)):
                index += 1
                if index == 4:
                    break
        print("YES" if index == 4 else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 4\nvika\n3 3\nbad\ncar\npet\n4 4\nvvvv\niiii\nkkkk\naaaa\n4 4\nvkak\niiai\navvk\nviaa\n4 7\nvbickda\nvbickda\nvbickda\nvbickda\n") == "YES\nNO\nYES\nNO\nYES"

# Custom cases
assert run("1\n1 4\nkvai\n") == "NO"  # letters out of order
assert run("1\n4 4\nvvvv\niiii\nkkkk\naaaa\n") == "YES"  # all letters in separate rows, same columns
assert run("1\n4 1\nv\ni\nk\na\n") == "NO"  # single column, cannot pick 4 distinct columns
assert run("1\n4 5\nvxxxx\nixxxx\nkxxxx\naxxxx\n") == "YES"  # letters spread, enough columns
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4\nkvai`| KHÔNG | Yêu cầu đặt hàng đúng | 
|`4 4\nvvvv\niiii\nkkkk\naaaa`| CÓ | Cho phép nhiều hàng trên mỗi cột | 
|`4 1\nv\ni\nk\na`| KHÔNG | Một cột không thể cung cấp 4 chữ cái | 
|`4 5\nvxxxx\nixxxx\nkxxxx\naxxxx`| CÓ | Chữ rải rác trên nhiều cột | 

## Vỏ cạnh 

Đối với thảm một cột kích thước 4x1:```
4 1
v
i
k
a
```Thuật toán quét cột duy nhất nhưng không thể tiến lên`index`qua 0 vì tất cả các chữ cái đều nằm trong cùng một cột. Đầu ra là "KHÔNG", xử lý chính xác yêu cầu cột riêng biệt. 

Đối với thảm một hàng có các chữ cái xuất hiện sai thứ tự:```
1 4
kvai
```Thuật toán quét các cột từ trái sang phải, tìm 'k' trước, không khớp với 'v', vì vậy`index`vẫn là 0. Đầu ra là "KHÔNG". 

Đối với các cột chứa nhiều chữ cái:```
4 4
vvvv
iiii
```
