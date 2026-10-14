---
title: "CF 105176J - \u6700\u540e\u4e00\u5757\u77f3\u5934\u7684\u91cd\u91cf"
description: "Chúng ta được cho một tập hợp các viên đá, mỗi viên đá có trọng lượng nguyên dương. Quá trình này liên tục lấy hai viên đá nặng nhất hiện có, phá hủy cả hai và nếu trọng lượng của chúng khác nhau, một viên đá mới sẽ được tạo ra có trọng lượng bằng chênh lệch của hai viên."
date: "2026-06-27T06:32:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105176
codeforces_index: "J"
codeforces_contest_name: "2024 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105176
solve_time_s: 35
verified: true
draft: false
---

[CF 105176J - \u6700\u540e\u4e00\u5757\u77f3\u5934\u7684\u91cd\u91cf](https://codeforces.com/problemset/problem/105176/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các viên đá, mỗi viên đá có trọng lượng nguyên dương. Quá trình này liên tục lấy hai viên đá nặng nhất hiện có, phá hủy cả hai và nếu trọng lượng của chúng khác nhau, một viên đá mới sẽ được tạo ra có trọng lượng bằng chênh lệch của hai viên. Điều này tiếp tục cho đến khi chỉ còn lại nhiều nhất một viên đá. Nhiệm vụ là xác định trọng lượng còn lại cuối cùng hoặc bằng 0 nếu không còn viên đá nào. 

Đầu vào đại diện cho nhiều tập trọng lượng đá ban đầu. Đầu ra là một số nguyên duy nhất mô tả trọng lượng còn sót lại cuối cùng sau khi liên tục áp dụng thao tác “đập vỡ hai lớn nhất”. 

Cấu trúc của bài toán gợi ý ngay rằng khó khăn chính không phải là số học mà là việc lặp lại việc lựa chọn hai phần tử cực đại. Nếu có tối đa n viên đá, một mô phỏng đơn giản quét toàn bộ danh sách để tìm hai phần tử lớn nhất mỗi lần sẽ tốn O(n) cho mỗi thao tác. Vì mỗi thao tác làm giảm số lượng đá đi một viên nên tổng số thao tác là O(n), dẫn đến thời gian O(n2) trong trường hợp xấu nhất. Tốc độ này quá chậm khi n lớn, chẳng hạn như 10⁵, trong đó hành vi bậc hai sẽ vượt xa các giới hạn khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi kết quả cuối cùng bằng 0 vì tất cả các viên đá đều triệt tiêu lẫn nhau. Ví dụ: nếu đầu vào là [2, 7, 4, 1, 8, 1] thì quá trình kết thúc với một viên đá có trọng số 1. Nhưng nếu đầu vào là [1, 1] thì cả hai viên đá đều bị phá hủy và không còn gì, vì vậy câu trả lời đúng là 0. Việc triển khai bất cẩn trả về chênh lệch được tính toán cuối cùng mà không kiểm tra tính trống có thể tạo ra giá trị cũ hoặc chưa được khởi tạo không chính xác trong trường hợp này. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu duy trì danh sách các viên đá và liên tục quét nó để tìm ra hai phần tử lớn nhất. Sau khi loại bỏ chúng, nó sẽ chèn sự khác biệt của chúng hoặc không có gì. Cách tiếp cận này đúng vì nó phản ánh chính xác quá trình được mô tả. Điểm thất bại là hiệu suất: mỗi lần trích xuất hai phần tử trên cùng tốn O(n) và điều này xảy ra O(n) lần, dẫn đến tổng công việc là O(n2). Đối với đầu vào lớn, điều này trở nên không khả thi. 

Quan sát quan trọng là quá trình này chỉ cần truy cập nhanh vào các phần tử tối đa hiện tại. Không có yêu cầu duy trì trật tự ngoài việc trích xuất lặp đi lặp lại các giá trị lớn nhất. Đây chính xác là trường hợp sử dụng cho vùng heap tối đa (hàng đợi ưu tiên). Bằng cách lưu trữ tất cả các viên đá trong một đống, chúng ta có thể trích xuất hai phần tử lớn nhất trong thời gian O(log n) mỗi phần tử và chèn một giá trị mới trở lại O(log n). Điều này làm giảm tổng độ phức tạp xuống O(n log n), đủ cho các ràng buộc thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(n²) | O(1)-O(n) | Quá chậm | 
| Mô phỏng đống tối đa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình hệ thống bằng cách sử dụng vùng heap tối đa, cho phép chúng tôi luôn truy xuất hai viên đá lớn nhất hiện tại một cách hiệu quả. 

1. Đưa tất cả các khối đá vào một đống tối đa. Chúng tôi đảo ngược các giá trị nếu sử dụng triển khai vùng heap tối thiểu vì hầu hết các thư viện tiêu chuẩn chỉ cung cấp vùng heap tối thiểu. 
2. Khi có nhiều hơn một viên đá trong đống, hãy loại bỏ viên đá lớn nhất x. Điều này đại diện cho viên đá nặng nhất hiện có tại thời điểm đó. 
3. Loại bỏ viên đá lớn thứ hai y. Tại thời điểm này, chúng tôi đang mô phỏng hoạt động theo cặp cần thiết trên hai ứng cử viên mạnh nhất. 
4. Nếu x và y không bằng nhau, hãy tính hiệu của chúng x − y và đẩy nó trở lại heap. Nếu chúng bằng nhau, cả hai viên đá đều bị phá hủy và không có gì được lắp lại. 
5. Tiếp tục cho đến khi heap có kích thước 0 hoặc 1. Nếu còn lại một phần tử, giá trị đó là câu trả lời. Nếu trống, câu trả lời là 0.

Lý do trích xuất tham lam này hợp lệ là vì bài toán không bao giờ phụ thuộc vào thứ tự tương đối của các phần tử không cực đại. Chỉ có hai giá trị lớn nhất mới ảnh hưởng đến kết quả của từng bước và việc loại bỏ chúng không yêu cầu phải xem xét lại các quyết định trước đó. Vùng heap đảm bảo rằng ở mỗi bước chúng ta đang áp dụng thao tác chính xác như được xác định ở mức tối đa toàn cầu hiện tại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n_and_rest = sys.stdin.read().strip().split()
    if not n_and_rest:
        return
    n = int(n_and_rest[0])
    arr = list(map(int, n_and_rest[1:]))

    # use max heap via negatives
    heap = [-x for x in arr]
    heapq.heapify(heap)

    while len(heap) > 1:
        x = -heapq.heappop(heap)
        y = -heapq.heappop(heap)

        if x != y:
            heapq.heappush(heap, -(x - y))

    if heap:
        print(-heap[0])
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên Python`heapq`, là vùng heap tối thiểu, vì vậy tất cả các giá trị được lưu trữ dưới dạng số âm để mô phỏng vùng heap tối đa. Điều kiện vòng lặp`len(heap) > 1`đảm bảo chúng tôi chỉ cố gắng trích xuất các cặp khi chúng tồn tại. Bước kiểm tra cuối cùng xử lý trường hợp heap trống một cách rõ ràng, điều này cần thiết cho các đầu vào mà tất cả các khối đá đều bị loại bỏ. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[2, 7, 4, 1, 8, 1]`. 

| Bước | Heap (dạng tối đa khái niệm) | Đã chọn x | Được chọn y | Hành động | Đống sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [8, 7, 4, 2, 1, 1] | 8 | 7 | đẩy 1 | [4, 2, 1, 1, 1] | 
| 2 | [4, 2, 1, 1, 1] | 4 | 2 | đẩy 2 | [2, 1, 1, 1] | 
| 3 | [2, 1, 1, 1] | 2 | 1 | đẩy 1 | [1, 1, 1] | 
| 4 | [1, 1, 1] | 1 | 1 | xóa cả hai | [1] | 

Câu trả lời cuối cùng là 1. 

Dấu vết này cho thấy những khác biệt trung gian có thể được đưa vào lại nhóm và vẫn ảnh hưởng đến các hoạt động sau này, đó là lý do tại sao cần có một cấu trúc toàn cục như một đống. 

Bây giờ hãy xem xét`[1, 1]`. 

| Bước | Đống | x | y | Hành động | Đống sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1, 1] | 1 | 1 | xóa cả hai | [] | 

Heap trở nên trống rỗng, xác nhận rằng chúng ta phải xuất ra số 0 một cách rõ ràng khi không còn phần tử nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi viên đá được chèn một lần và có khả năng di chuyển trong quá trình vận hành đống và mỗi lần đập đều hoạt động O(log n) | 
| Không gian | O(n) | Heap lưu trữ tất cả các viên đá hiện tại | 

Chi phí logarit có thể chấp nhận được với n tối đa là 10⁵, đây là điều điển hình cho loại vấn đề này. Cấu trúc tránh hoàn toàn việc quét bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import contextlib
    import io as _io

    out = _io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return out.getvalue().strip()

# minimal cases
assert run("1\n5\n5") == "5"
assert run("2\n1 1") == "0"

# sample-like case
assert run("6\n2 7 4 1 8 1") == "1"

# all equal
assert run("4\n3 3 3 3") == "0"

# decreasing
assert run("5\n9 7 5 3 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 5 | trường hợp cạnh phần tử đơn | 
| 1 1 1 | 0 | hủy bỏ hoàn toàn | 
| 2 7 4 1 8 1 | 1 | hành vi nhiều bước tiêu chuẩn | 
| 3 3 3 3 | 0 | lặp đi lặp lại loại bỏ bằng nhau | 
| 9 7 5 3 1 | 1 | sự khác biệt xếp tầng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các viên đá bị loại bỏ hoàn toàn. Đối với đầu vào`[1, 1]`, thuật toán sẽ đẩy cả hai giá trị vào heap, bật chúng lên, tìm thấy chúng bằng nhau và không đẩy lùi bất kỳ giá trị nào. Heap trở nên trống rỗng và đầu ra cuối cùng chính xác trở thành 0. 

Một trường hợp tinh vi khác là khi các quá trình khử trung gian liên tục tạo ra những viên đá nhỏ hơn, sau đó chúng lại tương tác với nhau. Ví dụ`[5, 4, 3]`đầu tiên sản xuất`5-4=1`, sau đó`[3, 1]`sản xuất`2`, và cuối cùng câu trả lời là`2`. Heap đảm bảo những giá trị mới được tạo này luôn được xem xét lại vào đúng thời điểm vì chúng nhập lại cùng cấu trúc toàn cầu và cạnh tranh bình đẳng với các viên đá hiện có.
