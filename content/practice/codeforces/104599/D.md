---
title: "CF 104599D - Chia dữ liệu"
description: "Chúng ta được cung cấp một tập hợp các tệp, mỗi tệp có kích thước cố định tính bằng bit và ngân sách lưu trữ là $X$. Từ những tệp này, chúng tôi muốn chọn càng nhiều tệp càng tốt đồng thời đảm bảo tổng kích thước của các tệp đã chọn không vượt quá $X$."
date: "2026-06-30T02:59:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "D"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 56
verified: true
draft: false
---

[CF 104599D - Chia dữ liệu](https://codeforces.com/problemset/problem/104599/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các tệp, mỗi tệp có kích thước cố định tính bằng bit và ngân sách lưu trữ$X$. Từ những tệp này, chúng tôi muốn chọn càng nhiều càng tốt đồng thời đảm bảo tổng kích thước của các tệp được chọn không vượt quá$X$. Mỗi tệp có thể được lấy hoặc bỏ qua và không có lựa chọn một phần. 

Đầu vào mang lại$N$kích thước tệp theo sau là giới hạn lưu trữ$X$. Nhiệm vụ là xuất ra số lượng tệp tối đa mà chúng tôi có thể phù hợp với ngân sách. 

Cấu trúc ngay lập tức gợi ý vấn đề đóng gói, nhưng mục tiêu không phải là tối đa hóa kích thước hoặc giá trị tổng thể. Thay vào đó, chúng ta muốn tối đa hóa số lượng phần tử được chọn theo một ràng buộc về tổng. Điều đó làm thay đổi đáng kể chiến lược tối ưu. 

Với$N \le 100{,}000$, bất kỳ giải pháp nào thử tất cả các tập hợp con đều không thể thực hiện được vì nó tăng theo cấp số nhân. Ngay cả việc kiểm tra tất cả các kết hợp cũng sẽ yêu cầu theo thứ tự$2^N$hoạt động hoàn toàn không thể thực hiện được. Chiến lược tham lam dựa trên sắp xếp hoặc thời gian tuyến tính là những ứng cử viên thực tế duy nhất. 

Một số trường hợp đặc biệt quan trọng: 

Nếu tất cả các tập tin lớn hơn$X$, thì không thể chọn tệp nào và câu trả lời là 0. 

Nếu tất cả các tệp cực kỳ nhỏ, ví dụ tất cả đều bằng 1 và$X = 10^9$, thì câu trả lời chỉ đơn giản là$N$, vì chúng ta có thể lấy mọi thứ. 

Nếu có một tệp rất lớn và nhiều tệp nhỏ, cách tiếp cận tham lam ngây thơ là chọn các tệp lớn trước sẽ thất bại vì nó sẽ tiêu tốn ngân sách quá sớm và làm giảm số lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xem xét mọi tập hợp con của tệp, tính tổng kích thước của nó và theo dõi xem nó chứa bao nhiêu phần tử nếu tổng nằm trong$X$. Điều này đúng vì nó khám phá mọi khả năng, nhưng nó sẽ trở nên không sử dụng được ngay khi$N$phát triển vượt quá giới hạn nhỏ. Với$N = 100{,}000$, số lượng tập hợp con lớn về mặt thiên văn, và thậm chí$N = 40$đã gần đạt đến giới hạn mà các kỹ thuật liệt kê được tối ưu hóa có thể xử lý được. 

Quan sát quan trọng là thứ tự chúng tôi chọn tệp sẽ quan trọng nếu mục tiêu của chúng tôi là tối đa hóa số lượng. Nếu chúng ta chọn một tệp lớn trong khi có một tệp nhỏ hơn, chúng ta đang lãng phí dung lượng lẽ ra có thể hỗ trợ nhiều mục hơn. Điều này cho thấy rằng để tối đa hóa số lượng tệp, trước tiên chúng ta nên ưu tiên các tệp nhỏ hơn. 

Sau khi chúng tôi sắp xếp tất cả kích thước tệp theo thứ tự tăng dần, chiến lược tốt nhất sẽ trở nên đơn giản: lấy tệp theo thứ tự tăng dần cho đến khi việc thêm kích thước tệp tiếp theo sẽ vượt quá ngân sách. Sự lựa chọn tham lam này là an toàn vì mọi lựa chọn hợp lệ của$k$các tập tin luôn có thể được chuyển đổi thành một lựa chọn$k$các tập tin nhỏ nhất mà không làm tăng tổng số tiền. Đối số trao đổi đó đảm bảo rằng việc sắp xếp không làm mất các giải pháp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(1)$| Quá chậm | 
| Tối ưu (sắp xếp + quét tham lam) |$O(N \log N)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả kích thước tệp và giới hạn lưu trữ$X$. Đây chỉ là sự chuẩn bị đầu vào trước khi bắt đầu bất kỳ lý luận nào. 
2. Sắp xếp mảng kích thước file theo thứ tự không giảm. Lý do sắp xếp là vì các tệp nhỏ hơn luôn hiệu quả hơn về mặt "số lượng trên mỗi đơn vị lưu trữ", vì vậy chúng tôi muốn ưu tiên chúng. 
3. Khởi tạo hai biến: tổng dung lượng đã sử dụng và bộ đếm số lượng tệp đã được chọn. Cả hai đều bắt đầu từ con số 0. 
4. Lặp lại các kích thước tệp được sắp xếp từ nhỏ nhất đến lớn nhất. Ở mỗi bước, hãy kiểm tra xem việc thêm tệp hiện tại có giữ tổng số trong phạm vi không$X$. 
5. Nếu việc thêm file không vượt quá$X$, bao gồm nó: tăng tổng chạy và tăng bộ đếm. 
6. Nếu việc thêm file vượt quá$X$, dừng quá trình ngay lập tức. Vì mảng đã được sắp xếp nên mọi tệp sau này ít nhất cũng lớn bằng, do đó không có tệp nào trong số chúng có thể được đưa vào mà không vi phạm ràng buộc. 
7. Xuất bộ đếm làm câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một thuộc tính trao đổi đơn giản. Giả sử một giải pháp tối ưu chọn một số tập hợp$k$tập tin. Nếu bộ đó không phải là$k$nhỏ nhất thì tồn tại một tệp được chọn lớn hơn tệp không được chọn. Hoán đổi chúng không thể làm tăng tổng số tiền và có thể làm giảm tổng số tiền, nghĩa là giải pháp hoán đổi vẫn hợp lệ và ít nhất là tốt. Việc lặp lại quá trình này sẽ biến đổi bất kỳ giải pháp tối ưu nào thành một giải pháp bao gồm$k$phần tử nhỏ nhất. 

Vì vậy, cách tốt nhất để tối đa hóa số lượng file tương đương với việc lấy những file nhỏ nhất trước cho đến khi hết ngân sách. Tiền tố tham lam của mảng được sắp xếp luôn tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    arr = [int(input()) for _ in range(n)]
    
    arr.sort()
    
    total = 0
    cnt = 0
    
    for v in arr:
        if total + v <= x:
            total += v
            cnt += 1
        else:
            break
    
    print(cnt)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc$N$Và$X$, theo sau là tất cả các kích thước tệp. Việc sắp xếp rất quan trọng vì nó thiết lập thứ tự tham lam trong đó chúng ta xem xét các phần tử. 

Vòng lặp duy trì hai bất biến:`total`luôn bằng tổng số tệp đã chọn cho đến nay và`cnt`bằng số lượng tập tin đã được chọn. Việc ngắt sớm là an toàn vì một khi tệp quá lớn thì tất cả các tệp sau đó ít nhất cũng lớn bằng do sắp xếp. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng xem xét lại các tập tin bị bỏ qua. Điều đó hợp lệ vì một khi chúng tôi chuyển một tệp theo thứ tự được sắp xếp, thì việc sử dụng tệp đó sau này sẽ chỉ có thể thực hiện được bằng cách xóa các tệp nhỏ hơn, điều này sẽ không bao giờ làm tăng số lượng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 34
14
25
47
11
6
```Mảng được sắp xếp: [6, 11, 14, 25, 47] 

| Bước | Giá trị hiện tại | Tổng Chạy | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 6 | 1 | lấy | 
| 2 | 11 | 17 | 2 | lấy | 
| 3 | 14 | 31 | 3 | lấy | 
| 4 | 25 | 56 | 3 | dừng lại | 

Quá trình dừng lại khi 25 vượt quá giới hạn. Câu trả lời là 3, cho thấy lựa chọn tốt nhất là ba tệp khả thi nhỏ nhất. 

### Mẫu 2 

đầu vào:```
6 18
2 5 6 4 13 1
```Mảng được sắp xếp: [1, 2, 4, 5, 6, 13] 

| Bước | Giá trị hiện tại | Tổng Chạy | Đếm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | lấy | 
| 2 | 2 | 3 | 2 | lấy | 
| 3 | 4 | 7 | 3 | lấy | 
| 4 | 5 | 12 | 4 | lấy | 
| 5 | 6 | 18 | 5 | lấy | 
| 6 | 13 | 18 | 5 | bỏ qua (sẽ vượt quá) | 

Chúng tôi đã lấy thành công năm tệp trước khi đạt đến giới hạn chính xác. Điều này chứng tỏ rằng cách tiếp cận tham lam sẽ lấp đầy ngân sách một cách tự nhiên nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(1)$thêm (hoặc$O(N)$tùy vào nơi lưu trữ) | chỉ mảng đầu vào cộng với bộ đếm | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó$O(N \log N)$giải pháp sắp xếp phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB vì ​​chỉ có một mảng số nguyên được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, x = map(int, sys.stdin.readline().split())
    arr = [int(sys.stdin.readline()) for _ in range(n)]
    arr.sort()
    
    total = 0
    cnt = 0
    
    for v in arr:
        if total + v <= x:
            total += v
            cnt += 1
        else:
            break
    
    return str(cnt)

# provided samples
assert run("5 34\n14\n25\n47\n11\n6\n") == "3", "sample 1"
assert run("6 18\n2\n5\n6\n4\n13\n1\n") == "5", "sample 2"

# custom cases
assert run("1 10\n5\n") == "1"
assert run("3 3\n4\n5\n6\n") == "0"
assert run("4 10\n1\n1\n1\n1\n") == "4"
assert run("5 9\n9\n8\n7\n6\n5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tập tin nhỏ duy nhất | 1 | trường hợp tối thiểu trong đó luôn có thể lựa chọn | 
| tất cả đều quá lớn | 0 | không có lựa chọn khả thi | 
| đều nhỏ như nhau | 4 | sử dụng hết công suất | 
| giá trị lớn giảm dần | 1 | xác nhận việc sắp xếp là cần thiết | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các tệp vượt quá giới hạn. Ví dụ: đầu vào:```
3 5
10 20 30
```Sau khi sắp xếp, phần tử đầu tiên đã lớn hơn$X$. Vòng lặp ngay lập tức thất bại điều kiện và trả về 0. Điều này xác nhận rằng việc chấm dứt sớm hoạt động chính xác ngay cả khi không có gì có thể chọn được. 

Một trường hợp khác là khi giải pháp tối ưu yêu cầu lấy nhiều phần tử nhỏ thay vì một phần tử lớn. Ví dụ:```
4 10
6 6 6 6
```Thứ tự sắp xếp vẫn giữ nguyên. Phần tử 6 đầu tiên được lấy, nhưng 6 phần tử tiếp theo sẽ vượt quá ngân sách, vì vậy câu trả lời là 1. Mọi nỗ lực chọn nhiều phần tử đều không thành công vì mọi sự kết hợp của hai phần tử đều vượt quá giới hạn, phù hợp với kết quả tham lam.
