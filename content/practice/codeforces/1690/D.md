---
title: "CF 1690D - Sọc Đen Trắng"
description: "Chúng ta được cấp một sọc ô, mỗi ô có màu trắng hoặc đen và mục tiêu là đảm bảo rằng ít nhất một đoạn trong k ô liên tiếp có màu đen hoàn toàn."
date: "2026-06-09T23:17:48+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1690
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 797 (Div. 3)"
rating: 1000
weight: 1690
solve_time_s: 84
verified: true
draft: false
---

[CF 1690D - Sọc đen trắng](https://codeforces.com/problemset/problem/1690/D) 

**Đánh giá:** 1000 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dải ô, mỗi ô có màu trắng hoặc đen và mục tiêu là đảm bảo rằng ít nhất một đoạn có kích thước chính xác.`k`gạch liên tiếp có màu đen hoàn toàn. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định độ dài của sọc, độ dài đoạn được yêu cầu và trình tự các ô. Nhiệm vụ của chúng ta là tính số lượng gạch trắng tối thiểu cần sơn lại thành màu đen để đạt được đoạn chiều dài màu đen liền kề nhau.`k`. Nếu phân khúc như vậy đã tồn tại thì câu trả lời là không. 

Những ràng buộc cho phép`n`lớn tới 200.000 và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng giới hạn ở mức 200.000. Điều này ngụ ý rằng bất kỳ giải pháp nào có độ phức tạp kém hơn O(n) cho mỗi trường hợp thử nghiệm đều có thể sẽ hết thời gian chờ. Cách tiếp cận Naive O(n*k) quá chậm vì`k`có thể so sánh với`n`. 

Các trường hợp cạnh tinh vi phát sinh khi chiều dài đoạn yêu cầu bằng tổng chiều dài,`k = n`, hoặc khi`k = 1`. Ví dụ, một sọc`W`có độ dài 1 với`k=1`yêu cầu sơn lại một lần. Một kịch bản khác là một sọc đen dài`n`với`k<n`; câu trả lời là số không. Việc xử lý không chính xác các trường hợp đặc biệt này thường xuất phát từ các lỗi riêng lẻ trong việc lập chỉ mục phân đoạn hoặc do không xem xét các phân đoạn bắt đầu ở vị trí đầu tiên hoặc cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: kiểm tra mọi đoạn chiều dài liền kề có thể`k`, đếm số ô trắng trong đó và lấy số nhỏ nhất. Điều này hoạt động vì mỗi phân đoạn là độc lập và số lần sơn lại chính xác là số ô màu trắng trong phân đoạn đó. Tuy nhiên, lực lượng vũ phu có độ phức tạp O(n_k) vì đối với mỗi phần tử đại khái`n-k+1`các phân đoạn, chúng tôi lặp đi lặp lại`k`gạch lát. Trong trường hợp xấu nhất của`n = 2*10^5`Và`k ≈ n`, điều này sẽ dẫn đến khoảng 4_10^10 thao tác, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là vấn đề thể hiện thuộc tính cửa sổ trượt. Số lượng gạch trắng trên một đoạn dài`k`thay đổi có thể đoán trước được khi chúng ta di chuyển cửa sổ sang bên phải một vị trí: chúng ta trừ đi phần đóng góp của ô rời khỏi cửa sổ và cộng phần đóng góp của ô đi vào. Điều này cho phép chúng tôi tính toán số lượng gạch trắng trong mỗi đoạn có chiều dài`k`trong thời gian O(n). 

Quan sát cho thấy tổng số màu trắng trong cửa sổ trượt có thể được cập nhật tăng dần sẽ làm giảm nghiệm từ O(n*k) xuống O(n). Chúng ta chỉ cần duy trì số lượng người da trắng đang chạy và cập nhật nó khi cửa sổ di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`k`, và chuỗi sọc. 
3. Khởi tạo bộ đếm`current_whites`cho cửa sổ đầu tiên có chiều dài`k`bằng cách đếm số ô màu trắng ở các vị trí từ 0 đến k-1. 
4. Đặt`min_whites`ĐẾN`current_whites`. 
5. Trượt cửa sổ từ vị trí 1 đến n-k. Đối với mỗi bước, nếu ô rời khỏi cửa sổ có màu trắng, hãy giảm`current_whites`. Nếu ô vào cửa sổ có màu trắng, hãy tăng`current_whites`. 
6. Cập nhật`min_whites`đến mức tối thiểu của chính nó và`current_whites`. 
7. Sau khi trượt qua toàn bộ dải, xuất ra`min_whites`là số lần sơn lại tối thiểu cần thiết. 

Tại sao nó hoạt động: Tính bất biến của cửa sổ trượt đảm bảo rằng`current_whites`luôn phản ánh chính xác số lượng gạch trắng trong đoạn chiều dài hiện tại`k`. Bằng cách lặp lại tất cả các phân đoạn chính xác một lần và theo dõi mức tối thiểu, chúng tôi tìm thấy số lần sơn lại tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    
    # initial window count
    current_whites = sum(1 for c in s[:k] if c == 'W')
    min_whites = current_whites
    
    for i in range(1, n - k + 1):
        if s[i-1] == 'W':
            current_whites -= 1
        if s[i+k-1] == 'W':
            current_whites += 1
        min_whites = min(min_whites, current_whites)
    
    print(min_whites)
```Vòng lặp đầu tiên đếm người da trắng trong đoạn ban đầu. Trượt cửa sổ từng vị trí một sẽ cập nhật số đếm một cách hiệu quả. Xử lý ranh giới là rất quan trọng: chỉ mục đầu tiên rời khỏi cửa sổ là`i-1`, và ô mới nhập là`i+k-1`. sử dụng`sum(1 for c in ...)`tránh được những sai sót nhỏ khi đếm lòng trắng. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`n=5, k=3, s="BBWBW"`| Bắt đầu cửa sổ | Nội dung cửa sổ | Người da trắng | min_whites | 
| --- | --- | --- | --- | 
| 0 | Bbw | 1 | 1 | 
| 1 | BWB | 2 | 1 | 
| 2 | WBW | 2 | 1 | 

Thuật toán nhận thấy đúng rằng chỉ cần sơn lại 1 ô là đủ. 

**Ví dụ 2**:`n=5, k=5, s="BBWBW"`| Bắt đầu cửa sổ | Nội dung cửa sổ | Người da trắng | min_whites | 
| --- | --- | --- | --- | 
| 0 | BBWBW | 2 | 2 | 

Chỉ có một cửa sổ tồn tại nên cần phải sơn lại 2 lần. Thuật toán xử lý cạnh của chiều dài cửa sổ bằng chiều dài sọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý tối đa hai lần: một lần vào và một lần thoát khỏi cửa sổ trượt | 
| Không gian | O(1) | Chỉ có một số quầy được sử dụng; không có mảng bổ sung tỷ lệ thuận với n | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2*10^5, giải pháp sẽ chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        current_whites = sum(1 for c in s[:k] if c == 'W')
        min_whites = current_whites
        for i in range(1, n - k + 1):
            if s[i-1] == 'W':
                current_whites -= 1
            if s[i+k-1] == 'W':
                current_whites += 1
            min_whites = min(min_whites, current_whites)
        print(min_whites)
    
    return output.getvalue().strip()

# provided samples
assert run("4\n5 3\nBBWBW\n5 5\nBBWBW\n5 1\nBBWBW\n1 1\nW\n") == "1\n2\n0\n1", "sample tests"

# custom cases
assert run("3\n1 1\nB\n3 2\nWWW\n6 3\nBBBWBB\n") == "0\n2\n0", "edge cases"
assert run("2\n5 5\nBBBBB\n4 2\nWBWB\n") == "0\n1", "full black and alternating"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 B | 0 | sọc dài tối thiểu đã có màu đen | 
| 3 2 WWW | 2 | tất cả người da trắng yêu cầu sơn lại nhiều lần | 
| 6 3 BBBBBB | 0 | đoạn có độ dài k đã tồn tại | 
| 5 5 BBBBB | 0 | toàn bộ sọc đã đen rồi, k=n | 
| 4 2 WBWB | 1 | kiểu xen kẽ, cửa sổ trượt | 

## Vỏ cạnh 

cho`n=1, k=1, s="W"`, cửa sổ ban đầu chứa một ô màu trắng. Thuật toán tính`current_whites=1`,`min_whites=1`, và in ra 1. Điều này đúng: chúng ta phải sơn lại ô duy nhất. Không có lỗi chỉ mục nào xảy ra do vòng lặp cửa sổ trượt không chạy khi`n-k=0`. 

Vì`n=5, k=5, s="BBBBB"`, cửa sổ duy nhất đã không chứa người da trắng.`current_whites=0`,`min_whites=0`. Thuật toán xuất ra 0 một cách chính xác mà không cần vào vòng lặp. 

Đối với các mẫu xen kẽ như`WBWB`,`current_whites`bắt đầu từ 1 (đối với cửa sổ đầu tiên). Trượt các bản cập nhật cửa sổ sẽ đếm chính xác, xác nhận rằng các bản cập nhật gia tăng xử lý sự phân bố khác nhau của người da trắng và người da đen.
