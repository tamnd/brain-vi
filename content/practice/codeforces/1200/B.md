---
title: "CF 1200B - Phiêu Lưu Khối"
description: "Trong Block Adventure, chúng ta có một hàng cột có độ cao khác nhau. Người chơi bắt đầu ở cột đầu tiên và phải đến cột cuối cùng."
date: "2026-06-11T23:53:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1200
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 578 (Div. 2)"
rating: 1200
weight: 1200
solve_time_s: 110
verified: true
draft: false
---

[CF 1200B - Phiêu lưu khối](https://codeforces.com/problemset/problem/1200/B) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong Block Adventure, chúng ta có một hàng cột có độ cao khác nhau. Người chơi bắt đầu ở cột đầu tiên và phải đến cột cuối cùng. Nhân vật có thể mang các khối trong túi và sửa đổi độ cao của cột bằng cách xóa một khối khỏi cột hiện tại hoặc đặt một khối từ túi lên đó. Việc di chuyển sang cột tiếp theo chỉ có thể thực hiện được nếu chênh lệch chiều cao không vượt quá giới hạn nhất định,`k`. Mục đích là để xác định xem người chơi có thể đi từ cột đầu tiên đến cột cuối cùng theo các quy tắc này hay không. 

Đầu vào đưa ra số cột, số khối ban đầu trong túi, dung sai`k`và một mảng chiều cao của cột. Đầu ra là "CÓ" nếu người chơi có thể đến cột cuối cùng và "KHÔNG" nếu ngược lại. 

Những ràng buộc chỉ ra rằng`n`nhiều nhất là 100, đủ nhỏ để xử lý từng cột trong một vòng lặp đơn giản. Chiều cao của túi và cột có thể lên tới$10^6$, có nghĩa là chúng tôi không thể mô phỏng mọi hoạt động chuyển khối có thể xảy ra. Thay vào đó, chúng ta cần một cách tiếp cận tham lam để cập nhật linh hoạt số khối trong túi khi chúng ta tiến về phía trước. Ví dụ, một cách tiếp cận bất cẩn có thể cố gắng mô phỏng mọi khả năng sắp xếp lại các khối hoặc thất bại khi`k=0`hoặc người chơi có đủ số khối để thực hiện bước nhảy. 

Trường hợp cạnh phát sinh khi một cột đã thấp hơn cột tiếp theo nhưng túi không có đủ khối để lấp đầy khoảng trống. Ví dụ, với`h = [3, 7]`,`m = 3`, Và`k = 0`, chúng ta không thể đến cột tiếp theo vì khoảng cách vượt quá mức cho phép và việc sử dụng tất cả các khối trong túi vẫn sẽ để lại thâm hụt. Một sự kiểm tra ngây thơ chỉ`|h_i - h_{i+1}| <= k`sẽ gợi ý không chính xác rằng không thể di chuyển nếu không xem xét các khối có thể được thêm vào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi sự kết hợp có thể có của việc thêm hoặc xóa các khối ở mỗi cột, theo dõi nội dung trong túi. Mặc dù điều này đúng nhưng nó bùng nổ về mặt tổ hợp vì tại mỗi cột, chúng ta có thể thêm hoặc xóa nhiều khối, tạo ra một số lượng lớn trạng thái. Với`n=100`Và`m`có khả năng lên đến$10^6$, số lượng thao tác trở nên hoàn toàn không thực tế. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng mọi hành động. Điều duy nhất quan trọng ở mỗi bước là liệu chúng ta có thể đáp ứng chiều cao của cột tiếp theo trong phạm vi dung sai hay không`k`. Nếu cột hiện tại cao hơn mức cần thiết, chúng ta bỏ các khối thừa vào túi. Nếu ngắn hơn, chúng ta sử dụng các khối từ túi để đạt chiều cao tối thiểu cho phép. Cách tiếp cận tham lam này hoạt động vì túi có dung lượng không giới hạn và chúng tôi chỉ quan tâm đến việc tiếp cận cột tiếp theo chứ không phải trình tự chính xác của các thao tác thêm/xóa. 

Chúng tôi duy trì số lượng khối trong túi một cách linh hoạt và kiểm tra từng lần chuyển đổi xem cột tiếp theo có thể truy cập được hay không. Điều này làm giảm vấn đề khi quét tuyến tính qua các cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| O(n + m) | Quá chậm | 
| Tham lam / Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`,`m`,`k`và mảng chiều cao cột`h`. 
2. Khởi tạo số khối trong túi là`m`. 
3. Lặp qua các cột từ cột đầu tiên đến cột thứ hai đến cuối cùng. Đối với mỗi cột`i`, tính chiều cao tối đa mà chúng ta có thể để lại ở cột một cách an toàn`i`do đó việc chuyển sang cột`i+1`là có thể. Đây là`max(0, h[i+1] - k)`. 
4. Nếu`h[i]`lớn hơn hoặc bằng chiều cao yêu cầu tối thiểu này thì hãy cho các khối thừa vào túi. Phần dư thừa là`h[i] - min_required`. 
5. Nếu`h[i]`nhỏ hơn chiều cao yêu cầu tối thiểu, hãy tính xem chúng ta cần bao nhiêu khối từ túi. Nếu túi chứa ít khối hơn mức cần thiết thì không thể di chuyển được; phá vỡ và trả về "KHÔNG". Nếu không, hãy trừ các khối cần thiết ra khỏi túi. 
6. Sau khi xử lý tất cả các chuyển đổi, nếu chúng tôi không bao giờ hết khối, hãy in "CÓ". 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đảm bảo chiều cao cột nằm trong dung sai cho phép cho bước tiếp theo. Bằng cách sử dụng hoặc thu thập các khối một cách tham lam, chúng tôi duy trì một sự bất biến rằng túi luôn chứa chính xác các khối dư thừa có sẵn từ các cột trước đó. Điều này đảm bảo rằng nếu chúng ta không thể đáp ứng một cột với túi hiện tại thì không thể tiếp tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    h = list(map(int, input().split()))
    bag = m
    possible = True
    for i in range(n - 1):
        min_required = max(0, h[i+1] - k)
        if h[i] >= min_required:
            bag += h[i] - min_required
        else:
            need = min_required - h[i]
            if bag < need:
                possible = False
                break
            bag -= need
    print("YES" if possible else "NO")
```Mã này tuân theo chiến lược tham lam. Chúng tôi tính toán độ cao tối thiểu cần thiết để di chuyển về phía trước và điều chỉnh túi cho phù hợp. các`max(0, h[i+1] - k)`đảm bảo chúng tôi không bao giờ cố gắng để lại chiều cao âm. Vòng lặp dừng ngay lập tức khi túi không thể bù đắp phần thiếu hụt, tiết kiệm những tính toán không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:`3 0 1`và độ cao`4 3 5`. 

| tôi | h[i] | h[i+1] | phút_required | túi | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 3 | 2 | 0 | 4 >= 2 → túi += 2 → túi = 2 | 
| 1 | 3 | 5 | 4 | 2 | 3 < 4 → cần = 1 → túi -= 1 → túi = 1 | 

Vòng lặp kết thúc và`bag >= 0`, vì vậy đầu ra là "CÓ". 

**Ví dụ 2**: 

đầu vào:`3 1 2`và độ cao`1 4 7`. 

| tôi | h[i] | h[i+1] | phút_required | túi | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 2 | 1 | 1 < 2 → nhu cầu = 1 → túi = 0 | 
| 1 | 4 | 7 | 5 | 0 | 4 < 5 → cần = 1 → túi < cần → không thể | 

Đầu ra là "KHÔNG". 

Những dấu vết này cho thấy thuật toán duy trì tính bất biến của túi và xác định chính xác khả năng tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Chúng tôi xử lý từng cột chính xác một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng chiều cao cột cho từng trường hợp thử nghiệm | 

Được cho`n ≤ 100`Và`t ≤ 1000`, tổng số thao tác tối đa là 100.000, nằm trong giới hạn 1 giây. Việc tính toán túi và chiều cao là các phép toán số nguyên, do đó không phát sinh tắc nghẽn hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        h = list(map(int, input().split()))
        bag = m
        possible = True
        for i in range(n - 1):
            min_required = max(0, h[i+1] - k)
            if h[i] >= min_required:
                bag += h[i] - min_required
            else:
                need = min_required - h[i]
                if bag < need:
                    possible = False
                    break
                bag -= need
        output.append("YES" if possible else "NO")
    return "\n".join(output)

# provided samples
assert run("5\n3 0 1\n4 3 5\n3 1 2\n1 4 7\n4 10 0\n10 20 10 20\n2 5 5\n0 11\n1 9 9\n99\n") == "YES\nNO\nYES\nNO\nYES"

# custom cases
assert run("1\n1 0 0\n0\n") == "YES", "single column"
assert run("1\n2 0 0\n5 5\n") == "YES", "exactly equal columns, k=0"
assert run("1\n2 1 0\n5 6\n") == "YES", "need one block from bag"
assert run("1\n2 0 0\n5 6\n") == "NO", "cannot reach next column"
assert run("1\n3 1000000 1000000\n1000000 0 1000000\n") == "YES", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 0 0\n0\n`| CÓ | Trường hợp cạnh một cột | 
|`1\n2 0 0\n5 5\n`| CÓ | Độ cao bằng nhau, k=0 | 
| `1\n2 1 0\n5 6 | | |
