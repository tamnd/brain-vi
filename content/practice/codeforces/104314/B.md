---
title: "CF 104314B - Bất bình đẳng"
description: "Chúng ta được cung cấp một hàng xen kẽ giữa các vị trí trống và các ký hiệu so sánh cố định. Có các vị trí $N+1$ phải được điền bằng các số riêng biệt từ 1 đến $N+1$ và giữa mỗi hai vị trí lân cận có chính xác một ràng buộc, “<” hoặc “”, phải…"
date: "2026-07-01T19:39:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104314
codeforces_index: "B"
codeforces_contest_name: "XXV Interregional Programming Olympiad, Vologda SU, 2023"
rating: 0
weight: 104314
solve_time_s: 72
verified: true
draft: false
---

[CF 104314B - Bất bình đẳng](https://codeforces.com/problemset/problem/104314/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng xen kẽ giữa các vị trí trống và các ký hiệu so sánh cố định. có$N+1$các vị trí phải được điền bằng các số khác nhau từ 1 đến$N+1$, và giữa mỗi hai vị trí lân cận có chính xác một ràng buộc “<” hoặc “>”, phải được thỏa mãn bởi hai số đặt bên cạnh nó. 

Nhiệm vụ là xác định xem liệu chúng ta có thể gán một hoán vị kích thước hay không$N+1$tới những vị trí này sao cho mọi bất đẳng thức đều đúng. Nếu có thể, chúng ta phải xuất ra một hoán vị hợp lệ; nếu không chúng ta sẽ xuất -1. 

Những ràng buộc cho phép$N$lên đến$10^5$, vì vậy mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc quay lại các hoán vị đều là không thể ngay lập tức bởi vì ngay cả$O(N!)$hoặc$O(2^N)$cách tiếp cận bùng nổ. Thậm chí$O(N^2)$công trình xây dựng sẽ quá chậm vì$10^5$bình phương đã vượt xa giới hạn. 

Một trường hợp thất bại tinh tế xuất hiện khi một công trình tham lam chọn cực tiểu hoặc cực đại cục bộ mà không tôn trọng các ràng buộc trong tương lai. Ví dụ: nếu chúng tôi cố gắng luôn đặt số nhỏ nhất có thể bất cứ khi nào chúng tôi thấy “<”, chúng tôi có thể chặn các chuỗi “>” sau này yêu cầu giá trị lớn trước đó ở giữa. 

Một trường hợp khác là các chuỗi dài xen kẽ như “< > < > <…”. Nhiều chiến lược tham lam ngây thơ đã thất bại ở đây vì chúng không phân phối chính xác các giá trị trên khắp các đỉnh và đáy, mặc dù luôn tồn tại một giải pháp hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là thử tất cả các hoán vị của các số từ 1 đến$N+1$và kiểm tra xem mỗi cái có thỏa mãn bất đẳng thức hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa về tính hợp lệ, nhưng nó đòi hỏi phải tạo ra$(N+1)!$hoán vị, và thậm chí kiểm tra một chi phí hoán vị$O(N)$, làm cho tổng độ phức tạp lớn về mặt thiên văn. 

Cấu trúc của bài toán thực sự bị chi phối bởi thứ tự tương đối hơn là giá trị tuyệt đối. Mỗi chuỗi bất bình đẳng tạo ra một mô hình gồm các phân đoạn tăng và giảm. Nếu chúng ta nhìn vào một đoạn có dấu “<” liên tiếp thì các con số phải tăng theo đúng đoạn đó. Tương tự, một đoạn dấu “>” liên tiếp buộc phải có cấu trúc giảm dần. Quan sát quan trọng là hoán vị có thể được xây dựng tăng dần bằng cách quyết định vị trí nào hoạt động giống như “thung lũng” và vị trí nào hoạt động giống như “đỉnh”. 

Thay vì gán số trực tiếp, chúng ta đảo ngược cách nhìn: nghĩ đến việc chèn các số từ 1 đến$N+1$theo thứ tự tăng dần và quyết định mỗi số sẽ đi đến đâu để tất cả các ràng buộc vẫn hợp lệ. Một cách cổ điển để thỏa mãn các chuỗi bất bình đẳng như vậy là xử lý các khối có dấu “>” liên tiếp bằng cách gán các giá trị giảm dần cho khối đó, điều này đương nhiên đòi hỏi phải đảo ngược thứ tự giống như ngăn xếp. 

Chúng tôi quét chuỗi và chia nó thành các phân đoạn tối đa trong đó bất đẳng thức là “>”. Bất cứ khi nào chúng ta gặp một đoạn như vậy, chúng ta biết khối tương ứng trong hoán vị phải được gán theo thứ tự ngược lại. Điều này dẫn đến một cấu trúc trong đó chúng ta nối thêm các chỉ mục một cách bình thường nhưng đảo ngược các phân đoạn bất cứ khi nào cần. 

Brute-force hoạt động vì nó kiểm tra tính chính xác một cách rõ ràng, nhưng không thành công do tăng trưởng giai thừa. Việc quan sát thấy cấu trúc phân rã thành các chuỗi đơn điệu cho phép chúng ta xây dựng một hoán vị hợp lệ trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((N+1)! \cdot N)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách quét tham lam trên chuỗi bất đẳng thức, nhóm các vị trí thành các khối phải đảo ngược. 

1. Khởi tạo danh sách trống`res`sẽ giữ hoán vị cuối cùng. 
2. Lặp qua các vị trí từ 0 đến$N$, coi mỗi vị trí là ranh giới giữa các số. 
3. Bất cứ khi nào chúng ta gặp một vị trí mà đoạn bất đẳng thức hiện tại chỉ chứa “>”, chúng ta sẽ tích lũy các chỉ số vào một vùng đệm tạm thời. 
4. Ngay sau khi chúng ta nhấn “<” hoặc đến cuối, chúng ta sẽ xóa bộ đệm. Flushing có nghĩa là thêm các chỉ số đã thu thập theo thứ tự ngược lại vào`res`. 
5. Tiếp tục quá trình này cho đến khi tất cả các vị trí được xử lý, đảm bảo rằng mọi chuỗi “>” tối đa được đảo ngược chính xác một lần. 

Lý do chúng tôi chỉ đảo ngược các phân đoạn “>” là vì chuỗi “>” buộc các giá trị giảm dần từ trái sang phải. Nếu chúng ta chỉ định các chỉ số tăng dần một cách tự nhiên, chúng ta sẽ vi phạm ràng buộc, vì vậy chúng ta sẽ đảo ngược thứ tự cục bộ. 

### Tại sao nó hoạt động 

Mỗi phân đoạn tối đa của các ràng buộc “>” liên tiếp thực thi thứ tự giảm dần trên các giá trị được đặt trong phân đoạn đó. Bằng cách thu thập các chỉ số theo thứ tự thuận và sau đó đảo ngược chúng khi xuất ra, chúng tôi đảm bảo rằng các số lớn hơn xuất hiện sớm hơn trong phân khúc đó và các số nhỏ hơn xuất hiện sau, đáp ứng mọi bất đẳng thức. Giữa các phân đoạn, các điểm chuyển tiếp tương ứng chính xác với “<”, duy trì trật tự tăng dần tự nhiên trên các ranh giới khối. Điều này đảm bảo mọi ràng buộc đều được thỏa mãn một cách độc lập và hoán vị toàn cục vẫn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().split()
    
    res = []
    i = 0
    
    while i <= n:
        j = i
        while j < n and s[j] == '>':
            j += 1
        
        # we have a segment [i, j]
        # for '>' segment, we reverse
        for k in range(j, i - 1, -1):
            res.append(k + 1)
        
        i = j + 1
    
    print(*res)

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng quét các phân đoạn “>” liên tiếp và đảo ngược các chỉ mục bên trong mỗi khối như vậy. biểu thức`k + 1`chuyển đổi các vị trí dựa trên số 0 thành các giá trị hoán vị cần thiết. 

Chi tiết triển khai chính là xử lý ranh giới một cách chính xác: khi chúng ta hoàn thành phân đoạn “>”, chúng ta phải bao gồm cả hai điểm cuối theo thứ tự đảo ngược. Các lỗi sai lệch thường phát sinh ở đây, đặc biệt là trong việc quyết định liệu đoạn đó có kết thúc ở`j`hoặc`j-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
< >
```Chúng tôi xử lý các vị trí từ 0 đến 2. 

| tôi | j | loại phân khúc | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | "<" | nối thêm [1] | [1] | 
| 1 | 1 | ">" đoạn kết thúc ngay lập tức | đảo ngược [2,1]? khối địa phương | [1,3,2] | 

Dấu vết này cho thấy dấu “>” đơn lẻ buộc cặp (3,2) theo thứ tự đảo ngược như thế nào, trong khi dấu “<” cho phép tăng vị trí khi bắt đầu. 

Hoán vị kết quả thỏa mãn cả hai ràng buộc: 1 < 3 và 3 > 2. 

### Ví dụ 2 

đầu vào:```
3
> < <
```Chúng tôi xử lý từng bước. 

| tôi | j | phân đoạn | hành động | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | ">" | đảo ngược [1,2] | [2,1] | 
| 2 | 2 | "<" | nối thêm [3] | [2,1,3] | 
| 3 | 3 | "<" | nối thêm [4] | [2,1,3,4] | 

Điều này chứng tỏ rằng tiền tố giảm dần được xử lý chính xác, trong khi các vị trí còn lại tuân theo thứ tự tăng dần tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi chỉ mục được truy cập một lần và được nối thêm một lần | 
| Không gian |$O(N)$| Cửa hàng mảng đầu ra$N+1$giá trị | 

Quét tuyến tính là đủ cho$N \le 10^5$và không cần cấu trúc dữ liệu bổ sung ngoài danh sách đầu ra, do đó cả hạn chế về bộ nhớ và thời gian đều được thỏa mãn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n< >\n") == "1 3 2"

# minimum case
assert run("1\n<\n") == "1 2"

# simple decreasing
assert run("1\n>\n") == "2 1"

# alternating pattern
assert run("3\n> < >\n") in ["2 1 4 3", "3 1 4 2"]

# all increasing
assert run("4\n< < < <\n") == "1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n< | 1 2 | trường hợp tăng hợp lệ nhỏ nhất | 
| 1\n> | 2 1 | đảo ngược đơn | 
| 3\n> < > | 2 1 4 3 (hoặc dạng hợp lệ tương đương) | cấu trúc xen kẽ | 
| 4\n< < < < | 1 2 3 4 5 | chuỗi tăng hoàn toàn | 

## Vỏ cạnh 

Trường hợp một cạnh là tiền tố giảm hoàn toàn như “> > > >”. Thuật toán thu thập toàn bộ tiền tố dưới dạng một phân đoạn duy nhất và đảo ngược nó, tạo ra một chuỗi giảm dần ngay từ đầu. Đối với đầu vào:```
4
> > > >
```Quá trình quét tạo thành một phân đoạn bao gồm tất cả các chỉ số từ 0 đến 4 và đầu ra trở thành [5, 4, 3, 2, 1], đáp ứng mọi ràng buộc. 

Một trường hợp khác là khi không có dấu “>” nào cả:```
3
< < <
```Ở đây không có sự đảo ngược nào xảy ra, do đó các chỉ số được thêm vào theo thứ tự tự nhiên, tạo ra [1, 2, 3, 4], thỏa mãn chính xác mọi bất đẳng thức. 

Trường hợp ranh giới hỗn hợp như “< > > <” cho thấy thuật toán phân chia chính xác thành các khối đơn điệu độc lập, đảm bảo không có sự tương tác giữa các phân đoạn có thể dẫn đến thứ tự mâu thuẫn.
