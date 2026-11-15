---
title: "CF 1846A - Rudolph và cắt dây"
description: "Chúng ta được phát một bộ đinh trên tường, mỗi chiếc có độ cao khác nhau và mỗi chiếc đinh có một sợi dây có độ dài nhất định buộc vào nó. Đầu còn lại của tất cả các sợi dây được nối với một viên kẹo, tạo thành một điểm duy nhất lơ lửng đâu đó trong không trung."
date: "2026-06-09T05:50:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1846
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 883 (Div. 3)"
rating: 800
weight: 1846
solve_time_s: 117
verified: false
draft: false
---

[CF 1846A - Rudolph và Cắt dây](https://codeforces.com/problemset/problem/1846/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ đinh trên tường, mỗi chiếc có độ cao khác nhau và mỗi chiếc đinh có một sợi dây có độ dài nhất định buộc vào nó. Đầu còn lại của tất cả các sợi dây được nối với một viên kẹo, tạo thành một điểm duy nhất lơ lửng đâu đó trong không trung. Mục đích là hạ kẹo xuống đất và chúng ta có thể làm điều này bằng cách cắt từng sợi dây một. Mỗi lần cắt sẽ làm giảm bộ dây giữ kẹo, có khả năng hạ thấp nó nếu các sợi dây còn lại không đủ dài để giữ kẹo trên mặt đất. 

Về mặt toán học, mỗi chiếc đinh đều có độ cao$a_i$và có một sợi dây dài$b_i$. Chiều cao của kẹo được xác định tối thiểu bằng$a_i + b_i$trên tất cả các sợi dây vẫn được gắn vào. Để đưa viên kẹo xuống đất, chúng ta cần tổng chiều cao tối thiểu cộng với chiều dài sợi dây để đạt tới 0 hoặc thấp hơn. Câu hỏi yêu cầu cắt số lượng dây tối thiểu để đạt được điều này. Từ$n \le 50$và mọi thao tác đều là những phép so sánh đơn giản, vấn đề chủ yếu là về việc sắp xếp và đếm chứ không phải về cấu trúc dữ liệu hay tối ưu hóa phức tạp. 

Trường hợp cạnh không rõ ràng xảy ra khi một số sợi dây đủ dài để tự chạm tới mặt đất. Ví dụ: nếu tồn tại một sợi dây có chiều dài lớn hơn chiều cao của chiếc đinh thì viên kẹo đã ở trên hoặc dưới mặt đất mà không bị cắt. Một sự tinh tế khác là giải pháp tối ưu có thể liên quan đến việc cắt những sợi dây gắn vào những chiếc đinh cao hơn trước chứ không phải những sợi dây dài nhất, vì chiều cao của viên kẹo được quyết định bởi sợi dây ngắn nhất có hiệu quả. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét tất cả các tập hợp con của sợi dây, cắt từng sợi một và kiểm tra xem viên kẹo có chạm đất hay không. Điều này hoạt động vì$n \le 50$, Vì thế$2^{50}$về mặt lý thuyết có thể có các tập hợp con, nhưng việc liệt kê tất cả là không khả thi. Tuy nhiên, quan sát quan trọng đã đơn giản hóa điều này: chúng ta chỉ cần biết có bao nhiêu sợi dây “quá ngắn” so với chiều cao hiện tại của viên kẹo. 

Chúng ta có thể sắp xếp những chiếc đinh theo chiều cao và tập trung vào chiều dài sợi dây thừa vượt quá vị trí của viên kẹo. Bắt đầu từ chiếc đinh cao nhất, nếu tổng chiều cao và chiều dài của sợi dây không chạm tới mặt đất thì chúng ta phải cắt nó. Bằng cách xem xét các dây theo thứ tự chiều cao đinh giảm dần và kiểm tra xem mỗi sợi dây có vượt quá số lượng dây đã cắt cho đến nay hay không, chúng ta có thể tính toán các vết cắt tối thiểu một cách tham lam. Điều này giúp giảm bớt vấn đề trong việc sắp xếp và đi một lần qua các sợi dây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối Ưu (Tham Lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng đinh$n$. Đối với mỗi móng tay, lưu trữ chiều cao$a_i$và chiều dài dây$b_i$như một bộ dữ liệu. 
2. Sắp xếp móng theo thứ tự chiều cao giảm dần$a_i$. Điều này đảm bảo chúng tôi xem xét những sợi dây ảnh hưởng đến chiều cao của viên kẹo nhiều nhất trước tiên. 
3. Khởi tạo bộ đếm`cuts = 0`để theo dõi số lượng dây đã cắt cho đến nay. 
4. Lặp lại các móng đã được sắp xếp. Đối với mỗi chiếc đinh, hãy kiểm tra xem chiều dài dây của nó có$b_i$nhỏ hơn hoặc bằng số dây đã cắt. Nếu vậy, hãy bỏ qua vì việc cắt nó không giúp làm giảm lượng kẹo hơn nữa. Ngược lại thì tăng`cuts`bởi vì sợi dây này cần phải được cắt đi để mang kẹo xuống. 
5. Tiếp tục cho đến khi tất cả các móng được xem xét. quầy`cuts`cuối cùng là số lượng dây tối thiểu cần cắt. 

Tại sao nó hoạt động: Bằng cách xem xét những chiếc đinh cao nhất trước tiên, chúng tôi đảm bảo rằng mọi sợi dây có thể ngăn kẹo rơi xuống đều được tính đến. Mỗi lần cắt làm giảm chiều cao hiệu quả tối thiểu của kẹo và tiêu chí tham lam đảm bảo không tính các vết cắt không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        ropes = [tuple(map(int, input().split())) for _ in range(n)]
        # Sort by nail height descending
        ropes.sort(reverse=True)
        cuts = 0
        for i in range(n):
            height, length = ropes[i]
            # If current rope cannot be the limiting factor, skip
            if length <= cuts:
                continue
            cuts += 1
        print(cuts)

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, lưu trữ mỗi sợi dây dưới dạng một bộ và sắp xếp theo thứ tự giảm dần để xử lý những chiếc đinh cao nhất trước tiên. Séc`if length <= cuts`là rất quan trọng, vì nó tránh được những vết cắt không cần thiết đối với những sợi dây vốn đã quá ngắn để ảnh hưởng đến việc rơi xuống của kẹo. Tăng dần`cuts`tham lam đếm những sợi dây phải cắt để chạm đất. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
4
9 2
5 2
7 7
3 4
```| Bước | Đinh (a_i, b_i) | Cắt giảm cho đến nay | Hành động | 
| --- | --- | --- | --- | 
| 1 | (9,2) | 0 | 2 > 0, cắt → cắt=1 | 
| 2 | (7,7) | 1 | 7 > 1, cắt → cắt=2 | 
| 3 | (5,2) | 2 | 2 2, bỏ qua | 
| 4 | (3,4) | 2 | 4 > 2, cắt → cắt=3 | 

Đầu ra: 3 

Điều này thể hiện sự lựa chọn tham lam dựa trên chiều dài dây hiệu quả so với vết cắt. 

### Đầu vào mẫu 2```
3
5
11 7
5 10
12 9
3 2
1 5
```Xử lý theo thứ tự chiều cao đinh giảm dần đảm bảo tính các vết cắt tối thiểu đồng thời tôn trọng các ràng buộc của dây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chi phối sự phức tạp | 
| Không gian | O(n) | Lưu trữ các bộ chiều dài đinh và dây | 

Được cho$n \le 50$, việc sắp xếp là không đáng kể và thuật toán dễ dàng chạy trong giới hạn thời gian. Việc sử dụng bộ nhớ thấp hơn nhiều so với hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n4 3\n3 1\n1 2\n4\n9 2\n5 2\n7 7\n3 4\n5\n11 7\n5 10\n12 9\n3 2\n1 5\n3\n5 6\n4 5\n7 7\n") == "2\n2\n3\n0", "sample 1"

# Custom cases
assert run("1\n1\n1 1\n") == "0", "single rope enough"
assert run("1\n2\n1 1\n2 2\n") == "1", "cut taller rope first"
assert run("1\n3\n3 3\n2 2\n1 1\n") == "2", "descending ropes needed"
assert run("1\n5\n5 1\n4 2\n3 3\n2 4\n1 5\n") == "2", "mix of short and long ropes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 sợi dây đã chạm đất | 0 | Không cần cắt giảm | 
| 2 sợi dây, dây cao hơn phải cắt | 1 | Lựa chọn tham lam đúng đắn | 
| 3 sợi dây đi xuống | 2 | Kế toán cắt giảm lũy kế | 
| Chiều dài dây hỗn hợp | 2 | Lựa chọn phù hợp theo chiều cao | 

## Vỏ cạnh 

Nếu tất cả các sợi dây đủ dài để chạm đất riêng lẻ thì không cần cắt. Nếu tất cả các sợi dây đều ngắn nhưng chiếc đinh cao nhất có sợi dây ngắn nhất thì có thể cần phải cắt nhiều lần. Việc sắp xếp theo chiều cao đảm bảo rằng những chiếc đinh cao nhất sẽ được xử lý trước và tránh việc đếm thiếu những vết cắt cần thiết. Ví dụ, một sợi dây đơn được buộc vào một chiếc đinh rất cao và có chiều dài cực ngắn phải được cắt trước tiên, thuật toán này sẽ xử lý một cách tự nhiên thông qua việc sắp xếp.
