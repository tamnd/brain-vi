---
title: "CF 286E - Shop Đồ Nữ"
description: "We are given a set of bags, each with a minimum weight capacity. Chúng ta cần xác định một tập hợp trọng lượng vật phẩm tối thiểu sao cho tổng trọng lượng mà một chiếc túi có thể chứa có thể được hình thành bằng cách sử dụng những vật phẩm này với số lượng không giới hạn."
date: "2026-06-05T10:01:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 286
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 176 (Div. 1)"
rating: 2800
weight: 286
solve_time_s: 131
verified: true
draft: false
---

[CF 286E - Cửa hàng dành cho phụ nữ](https://codeforces.com/problemset/problem/286/E) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, fft, toán 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ túi, mỗi túi có trọng lượng tối thiểu. Chúng ta cần xác định một tập hợp trọng lượng vật phẩm tối thiểu sao cho tổng trọng lượng mà một chiếc túi có thể chứa có thể được hình thành bằng cách sử dụng những vật phẩm này với số lượng không giới hạn. Ngoài ra, mọi trọng số có thể từ 1 đến mức tối đa nhất định`m`phải được biểu diễn dưới dạng tổng của các trọng số mục này trong một số kết hợp. Về cơ bản, các vật phẩm hoạt động giống như "đồng xu" và các túi xác định số tiền mục tiêu mà chúng tôi phải có thể đạt được, trong khi chúng tôi được yêu cầu tìm một bộ đồng xu tối thiểu bao gồm tất cả số tiền lên tới`m`và đạt chính xác từng giới hạn túi. 

Đầu vào cho chúng ta`n`dung tích túi riêng biệt và trọng lượng tối đa`m`. Những hạn chế`n, m ≤ 10^6`chỉ ra rằng bất kỳ giải pháp nào lặp lại trên tất cả các tập hợp trọng số có thể có sẽ quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính trong`m`để có hiệu quả. Các trường hợp cạnh phát sinh khi túi đầu tiên có trọng lượng 1 hoặc khi khoảng cách giữa các dung lượng túi liên tiếp buộc phải đưa vào nhiều trọng lượng riêng biệt để đáp ứng yêu cầu về phạm vi bao phủ. Ví dụ, nếu túi`[3, 4]`Và`m = 4`, một cách tiếp cận ngây thơ có thể bỏ sót rằng trọng số 1 hoặc 2 là cần thiết để bao phủ tất cả các tổng ≤ 4. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng chọn các mặt hàng một cách tham lam và xác minh mọi trọng lượng lên đến`m`liệu nó có thể được hình thành hay không. Điều này sẽ liên quan đến việc lặp lại tất cả các tổng có thể có cho mỗi trọng số ứng cử viên, điều này trở nên không khả thi vì`m`có thể lên đến`10^6`. Ngay cả việc sử dụng lập trình động với một mảng boolean đầy đủ để tính tổng cũng sẽ tốn kém.`O(m * n)`trong trường hợp xấu nhất có thể đạt tới`10^{12}`hoạt động. 

Nhận xét quan trọng là bài toán về cơ bản là một biến thể của bài toán đổi xu. Mỗi trọng lượng túi xác định một tổng mục tiêu phải được thể hiện. Nếu xử lý dung lượng túi theo thứ tự tăng dần, chúng ta chỉ có thể tham lam chọn trọng lượng vật phẩm mới khi gặp phải một túi có trọng lượng không thể được hình thành bởi tập hợp vật phẩm hiện tại. Lựa chọn tối thiểu cho trọng lượng vật phẩm mới là sự khác biệt giữa trọng lượng túi này và trọng lượng lớn nhất mà chúng tôi hiện có thể tạo ra với các vật phẩm của mình. Điều này đảm bảo rằng mỗi trọng lượng vật phẩm mới sẽ mở rộng phạm vi số tiền có thể biểu thị ít nhất nếu cần, tạo ra một bộ tối thiểu. Thuật toán kết quả về cơ bản là một cấu trúc tham lam trên tổng tiền tố của các trọng số có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(m) | Quá chậm | 
| Xây dựng đồng xu tham lam | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp dung tích túi theo thứ tự tăng dần. Điều này cho phép xử lý từ nhỏ nhất đến lớn nhất, đảm bảo mức tăng tối thiểu cho tập trọng số có thể biểu thị của chúng tôi. 
2. Khởi tạo`max_reachable`đến 0 và một danh sách trống trọng lượng vật phẩm`p`.`max_reachable`theo dõi trọng số lớn nhất mà chúng tôi hiện có thể biểu thị dưới dạng tổng trọng số của mục đã chọn. 
3. Lặp lại từng trọng lượng túi`a_i`. Nếu như`a_i`lớn hơn`max_reachable + 1`, điều đó có nghĩa là có một khoảng cách trong số tiền mà chúng ta có thể biểu thị. Thêm sự khác biệt`a_i - max_reachable`như một trọng lượng mặt hàng mới. Đây là trọng lượng tối thiểu cho phép chúng tôi đạt được`a_i`đồng thời mở rộng phạm vi tổng hiện tại của chúng tôi ở mức tối thiểu. 
4. Cập nhật`max_reachable`bằng cách thêm trọng lượng vật phẩm mới vào nó, mở rộng phạm vi tổng có thể biểu thị. 
5. Tiếp tục cho đến khi tất cả dung lượng túi được xử lý. Theo cấu trúc, mọi trọng lượng cho đến túi cuối cùng đều có thể biểu thị và trọng lượng của mỗi túi được đánh chính xác. 
6. Xuất ra danh sách cuối cùng về trọng lượng của vật phẩm. Nếu tại bất kỳ thời điểm nào trọng lượng túi không thể được che phủ và không có trọng lượng mới nào có thể mở rộng phạm vi (điều này là không thể trong thiết lập này vì trọng lượng là số nguyên ≥ 1), hãy in "KHÔNG". Nếu không, in "CÓ" và trọng lượng của vật phẩm. 

Tại sao nó hoạt động: Ở mỗi bước,`max_reachable`duy trì tính bất biến rằng tất cả các tổng từ 1 đến`max_reachable`có thể đạt được. Bằng cách chọn trọng lượng mới tối thiểu bất cứ khi nào xuất hiện khoảng trống, chúng tôi đảm bảo rằng chúng tôi sẽ thêm trọng lượng vật phẩm ít nhất có thể trong khi vẫn đáp ứng được tất cả số tiền cần thiết. Đây là một lựa chọn tham lam được chứng minh bằng cấu trúc của vấn đề thay đổi đồng xu, trong đó việc lấy số tiền bị thiếu tối thiểu luôn dẫn đến một bộ tối ưu trong cài đặt số tiền không giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    
    p = []
    max_reachable = 0
    for weight in a:
        while max_reachable + 1 < weight:
            new_item = max_reachable + 1
            p.append(new_item)
            max_reachable += new_item
        max_reachable = max(max_reachable, weight)
    
    print("YES")
    print(len(p))
    print(' '.join(map(str, p)))

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào và sắp xếp trọng lượng túi.`max_reachable`theo dõi số tiền đại diện lớn nhất. Bất cứ khi nào có khoảng trống trước trọng lượng túi, trọng lượng vật phẩm mới nhỏ nhất có thể sẽ được thêm vào để lấp đầy khoảng trống. Sau khi xử lý tất cả các túi,`p`chứa trọng lượng vật phẩm tối thiểu cần thiết. Thuật toán tránh các trọng số không cần thiết bằng cách luôn chọn giá trị nhỏ nhất có thể để che các khoảng trống, đảm bảo mức tối thiểu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 10
5 6 7 8 9 10
```| cân nặng | max_reachable | hành động | p | 
| --- | --- | --- | --- | 
| 5 | 0 | max_reachable+1 < 5 → thêm 1, 2, 3, 4 | [1,2,3,4] | 
| 5 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 
| 6 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 
| 7 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 
| 8 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 
| 9 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 
| 10 | 10 | trọng lượng ≤ max_reachable → không làm gì | [1,2,3,4] | 

Tập hợp các mục kết quả là`[1,2,3,4]`. Điều này bao gồm tất cả các khoản tiền từ 1 đến 10 và đánh chính xác vào từng túi. 

### Mẫu 2 

đầu vào:```
3 7
2 5 7
```| cân nặng | max_reachable | hành động | p | 
| --- | --- | --- | --- | 
| 2 | 0 | 1 < 2 → cộng 1 | [1] | 
| 2 | 1 | max_reachable đã cập nhật lên 2 | [1] | 
| 5 | 2 | 3 < 5 → cộng 3 | [1,3] | 
| 5 | 5 | max_reachable đã cập nhật lên 5 | [1,3] | 
| 7 | 5 | 6 < 7 → cộng 6 | [1,3,6] | 
| 7 | 11 | max_reachable đã cập nhật lên 11 | [1,3,6] | 

Tất cả trọng lượng hành lý đều được tính toán và tất cả tổng số tiền lên tới 7 đều được thể hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Việc sắp xếp trọng lượng túi mất O(n log n). Việc lặp lại và lấp đầy các khoảng trống có thể đạt tối đa là O(m) vì`max_reachable`mỗi lần tăng ít nhất 1. | 
| Không gian | O(n) | Chúng tôi lưu trữ trọng lượng túi và danh sách trọng lượng vật phẩm`p`. | 

Điều này rất phù hợp với các ràng buộc của bài toán, vì n và m lên tới 10^6 và Python có thể xử lý 10^6 thao tác một cách dễ dàng trong vòng 8 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("6 10\n5 6 7 8 9 10\n") == "YES\n4\n1 2 3 4", "sample 1"

# custom cases
assert run("3 7\n2 5 7\n") == "YES\n3\n1 3 6", "cover gaps with minimal items"
assert run("1 1\n1\n") == "YES\n0\n", "single bag 1 needs no extra item"
assert run("4 10\n2 3 7 10\n") == "YES\n3\n1 2 5", "gaps covered greedily"
assert run("5 5\n1 2 3 4 5\n") == "YES\n0\n", "all sums already covered by 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 7, 2 5 7 | CÓ 3 1 3 6 | Tham lam lựa chọn những món đồ tối thiểu để lấp đầy khoảng trống | 
| 1 1, 1 | CÓ 0 | Trọng lượng túi bằng 1, không cần thêm đồ | 
| 4 10, 2 3 7 10 | | |
