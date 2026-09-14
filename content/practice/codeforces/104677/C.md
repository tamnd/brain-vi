---
title: "CF 104677C - Bên Darcy"
description: "Chúng tôi được phân cho một nhóm người, mỗi người cầm một số lát bánh. Nếu chiếc bánh được chia một cách hoàn hảo thì mỗi người sẽ nhận được số lát bằng nhau, vì tổng số lát đảm bảo chia hết cho số người."
date: "2026-06-29T09:11:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104677
codeforces_index: "C"
codeforces_contest_name: "Sugar Sweet \u2764\ufe0f"
rating: 0
weight: 104677
solve_time_s: 62
verified: true
draft: false
---

[CF 104677C - Các bữa tiệc của Darcy](https://codeforces.com/problemset/problem/104677/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phân cho một nhóm người, mỗi người cầm một số lát bánh. Nếu chiếc bánh được chia một cách hoàn hảo thì mỗi người sẽ nhận được số lát bằng nhau, vì tổng số lát đảm bảo chia hết cho số người. 

Nhiệm vụ là xác định xem có bao nhiêu người không phù hợp với tỷ lệ chia đều lý tưởng này. Nói cách khác, chúng tôi tính toán số lát trung bình của mỗi người và đếm xem có bao nhiêu cá nhân khác với giá trị đó. 

Đầu vào là một mảng số nguyên nhỏ và đầu ra là một số nguyên duy nhất: số vị trí mà giá trị không bằng giá trị trung bình. 

Vì cả số người và số lát đều tối đa là 10 nên kích thước đầu vào cực kỳ nhỏ. Điều này loại bỏ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài một lần truyền qua mảng. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị đều bằng nhau. Trong trường hợp đó, câu trả lời sẽ bằng không. Một trường hợp góc khác là khi chỉ có một người tồn tại, vì “trung bình” gần như là giá trị của họ, nên một lần nữa câu trả lời là 0. 

Một sai lầm ngây thơ có thể là tính toán lại hoặc mô phỏng các hoạt động phân phối lại thay vì so sánh trực tiếp với mức trung bình được tính toán. Điều đó sẽ không cần thiết và có thể gây ra lỗi làm tròn hoặc chia số nguyên nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là thử phân phối lại các lát cắt cho đến khi mọi người trở nên bình đẳng, sau đó đếm xem có bao nhiêu người đã thay đổi trong quá trình đó. Người ta có thể mô phỏng việc chuyển giao giữa các cặp cho đến khi đạt được trạng thái cân bằng. Tuy nhiên, mặc dù những ràng buộc ở đây rất nhỏ, cách tiếp cận đó về cơ bản là không cần thiết và che khuất cấu trúc của vấn đề. 

Quan sát quan trọng là giá trị đúng cuối cùng của mỗi người là cố định và được xác định duy nhất: nó chỉ đơn giản là tổng số chia cho N. Không cần phải mô phỏng bất kỳ quy trình phân phối nào. Khi đã biết giá trị mục tiêu này, vấn đề sẽ chuyển sang so sánh trực tiếp với từng phần tử của mảng. 

Điều này biến vấn đề từ một mô phỏng quá trình thành một nhiệm vụ tổng hợp và đếm đơn giản. Ý tưởng vũ lực hoạt động về mặt khái niệm vì sự phân phối lại cuối cùng dẫn đến tính đồng nhất, nhưng nó không thành công về mặt phương pháp vì nó đưa ra những thay đổi trạng thái bổ sung không liên quan đến câu hỏi cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k·N²) hoặc tệ hơn | O(N) | Quá chậm/không cần thiết | 
| So sánh trực tiếp | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Đọc số người N và danh sách số lát. 
2. Tính tổng của tất cả các lát cắt. Điều này thể hiện chiếc bánh đầy đủ được phân phối cho tất cả những người tham gia. 
3. Tính số lát mục tiêu cho mỗi người bằng tổng chia cho N. Số này được đảm bảo là số nguyên do tình trạng vấn đề. 
4. Khởi tạo bộ đếm về 0. 
5. Lặp lại số lát của mỗi người. 
6. Đối với mỗi người, hãy so sánh giá trị của họ với giá trị mục tiêu. 
7. Nếu các giá trị khác nhau, hãy tăng bộ đếm. 
8. Xuất giá trị bộ đếm cuối cùng. 

Mỗi so sánh trực tiếp trả lời xem người đó có đi chệch khỏi sự phân phối công bằng hay không, do đó không cần chuyển đổi trung gian. 

### Tại sao nó hoạt động 

Ràng buộc tổng đảm bảo rằng tồn tại một giá trị số nguyên duy nhất đại diện cho phần chia sẻ công bằng. Vì tính công bằng được xác định hoàn toàn bằng sự bình đẳng đối với hằng số dẫn xuất này, nên mọi trường hợp sai chính xác là một phần tử không bằng hằng số đó. Thuật toán dựa vào bất biến rằng giá trị đích là chính xác trên toàn cầu và không phụ thuộc vào thứ tự hoặc mối quan hệ cục bộ giữa các phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    target = sum(a) // n
    ans = 0
    
    for x in a:
        if x != target:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của thuật toán. Chi tiết quan trọng nhất là tính toán mục tiêu bằng cách sử dụng phép chia số nguyên sau khi tính tổng tất cả các phần tử. Vì bài toán đảm bảo tính chia hết nên không cần xử lý dấu phẩy động hoặc làm tròn. 

Vòng lặp chỉ đơn giản là đếm những điểm không khớp. Không cần phân loại hoặc lưu trữ thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 2 2 2
```Đầu tiên hãy tính tổng: 1 + 3 + 2 + 2 + 2 = 10. 

Mục tiêu mỗi người là 10/5 = 2. 

Bây giờ so sánh từng giá trị: 

| Chỉ mục | Giá trị | Mục tiêu | Cuộc thi đấu? | Quầy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | Không | 1 | 
| 2 | 3 | 2 | Không | 2 | 
| 3 | 2 | 2 | Có | 2 | 
| 4 | 2 | 2 | Có | 2 | 
| 5 | 2 | 2 | Có | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này xác nhận rằng thuật toán chỉ xác định chính xác các phần tử khác với mức trung bình toàn cầu. 

### Ví dụ 2 

đầu vào:```
4
4 4 4 4
```Tổng là 16, mục tiêu là 4. 

| Chỉ mục | Giá trị | Mục tiêu | Cuộc thi đấu? | Quầy | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | Có | 0 | 
| 2 | 4 | 4 | Có | 0 | 
| 3 | 4 | 4 | Có | 0 | 
| 4 | 4 | 4 | Có | 0 | 

Câu trả lời cuối cùng là 0. 

Điều này cho thấy thuật toán xử lý chính xác cấu hình cân bằng hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lượt để tính tổng và một lượt để đếm các giá trị không khớp | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với N ≤ 10, giá trị này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. Ngay cả đối với những ràng buộc lớn hơn nhiều, giải pháp vẫn có quy mô tuyến tính và vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))
    target = sum(a) // n
    ans = sum(1 for x in a if x != target)
    return str(ans)

# provided sample
assert run("5\n1 3 2 2 2\n") == "2"

# all equal
assert run("3\n2 2 2\n") == "0"

# single element
assert run("1\n7\n") == "0"

# two elements mismatch
assert run("2\n1 3\n") == "2"

# mixed case
assert run("4\n0 2 2 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1 3 2 2 2 | 2 | phân phối hỗn hợp cơ bản | 
| 3 2 2 2 | 0 | đã cân bằng | 
| 1 7 | 0 | trường hợp cạnh đơn phần tử | 
| 2 1 3 | 2 | cả hai đều sai so với ý nghĩa | 
| 4 0 2 2 2 | 1 | trường hợp sai lệch thưa thớt | 

## Vỏ cạnh 

Đối với một người đầu vào như`1\n5`, thuật toán tính tổng 5, mục tiêu 5 và không tìm thấy sự không khớp nào, tạo ra 0. Điều này xác nhận rằng định nghĩa về tính công bằng suy biến chính xác khi N = 1. 

Đối với một mảng đã thống nhất như`3\n2 2 2`, mục tiêu được tính toán là 2 và mọi so sánh đều thành công, do đó bộ đếm vẫn bằng 0 xuyên suốt. Điều này xác minh rằng thuật toán không gắn cờ sai các cấu hình bằng nhau do kiểm tra dư thừa hoặc vấn đề chia số nguyên.
