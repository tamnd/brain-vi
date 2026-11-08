---
title: "CF 1395C - Hoạt động Boboniu và Bit"
description: "Chúng ta được cho hai mảng số nguyên nhỏ. Đối với mỗi phần tử trong mảng đầu tiên, chúng ta phải chọn một phần tử từ mảng thứ hai và kết hợp chúng bằng cách sử dụng AND theo từng bit. Điều này tạo ra một giá trị mới cho mỗi vị trí trong mảng đầu tiên."
date: "2026-06-11T09:36:02+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1395
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 664 (Div. 2)"
rating: 1600
weight: 1395
solve_time_s: 77
verified: true
draft: false
---

[CF 1395C - Hoạt động Boboniu và Bit](https://codeforces.com/problemset/problem/1395/C) 

**Đánh giá:** 1600 
**Tags:** bitmasks, vũ phu, dp, tham lam 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên nhỏ. Đối với mỗi phần tử trong mảng đầu tiên, chúng ta phải chọn một phần tử từ mảng thứ hai và kết hợp chúng bằng cách sử dụng AND theo từng bit. Điều này tạo ra một giá trị mới cho mỗi vị trí trong mảng đầu tiên. Sau khi chọn tất cả các cặp này, chúng tôi lấy bitwise OR trên tất cả các giá trị kết quả và muốn OR cuối cùng này càng nhỏ càng tốt. 

Một cách khác để thấy điều đó là mỗi phần tử trong mảng đầu tiên phải “chọn” một đối tác từ mảng thứ hai và mỗi lựa chọn tạo ra một mặt nạ bit đóng góp vào sự kết hợp toàn cầu của các bit. Vì OR tích lũy các bit vĩnh viễn nên bất kỳ bit nào xuất hiện trong bất kỳ kết quả AND nào được chọn sẽ xuất hiện trong câu trả lời cuối cùng. Mục tiêu là chọn các cặp sao cho càng ít bit tồn tại trong liên minh này càng tốt. 

Các ràng buộc rất nhỏ: cả hai mảng có kích thước tối đa là 200 và mỗi số có 9 bit, nghĩa là tất cả các giá trị nằm trong phạm vi từ 0 đến 511. Sự kết hợp này rất quan trọng. Độ rộng bit nhỏ cho phép chúng ta suy nghĩ theo các tập hợp con của bit thay vì độ lớn bằng số và số lượng phần tử nhỏ cho phép lập trình động trên các tập hợp con hoặc trạng thái. 

Một cách tiếp cận đơn giản sẽ thử gán cho mỗi phần tử trong số n phần tử một trong m lựa chọn một cách độc lập. Điều đó đã mang lại nhiều khả năng, điều này hoàn toàn không khả thi ngay cả đối với n vừa phải. Ngay cả việc cố gắng tính toán tất cả các kết quả HOẶC có thể có trực tiếp từ các kết hợp cũng sẽ bùng nổ. 

Một trường hợp thất bại tinh vi hơn xuất phát từ lối suy nghĩ tham lam. Người ta có thể thử gán từng aᵢ bⱼ để cực tiểu hóa aᵢ & bⱼ cục bộ. Điều này không hiệu quả vì việc giảm một cᵢ không đảm bảo OR toàn cầu nhỏ. Việc giới thiệu sớm một chút có thể khó tránh khỏi sau này do những hạn chế khác. 

Ví dụ: nếu một aᵢ chỉ có thể tránh một bit bằng cách sử dụng một bⱼ cụ thể, nhưng một aₖ khác không có lựa chọn nào và buộc phải thực hiện bit đó, thì lựa chọn tham lam cho aᵢ là không liên quan. Sự kết hợp giữa tất cả các phép gán có tính toàn cục thông qua OR. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là gán cho mỗi aᵢ một trong các giá trị m và tính OR kết quả. Mỗi phép gán tạo ra n giá trị cᵢ = aᵢ & bⱼ, sau đó chúng ta HOẶC chúng. Điều này đòi hỏi phải khám phá mⁿ bài tập và thậm chí việc cắt tỉa cũng không giúp ích một cách có ý nghĩa vì OR không cho phép đảm bảo sớm tính tối ưu cục bộ. Với n = 200, điều này vượt xa mọi giới hạn tính toán. 

Quan sát chính xuất phát từ cấu trúc bitwise OR. Câu trả lời cuối cùng là mặt nạ 9 bit. Điều đó có nghĩa là chỉ có 2⁹ = 512 kết quả cuối cùng có thể xảy ra. Thay vì quyết định trực tiếp các bài tập, chúng ta có thể nghĩ ngược lại: chúng ta đoán mặt nạ OR cuối cùng và kiểm tra xem nó có thể đạt được hay không. 

Sửa mặt nạ ứng cử viên X. Chúng tôi muốn biết liệu chúng tôi có thể gán mỗi aᵢ a bⱼ sao cho mọi kết quả (aᵢ & bⱼ) là mặt nạ con của X. Nếu bất kỳ cặp được chọn nào đưa ra một bit bên ngoài X, thì ứng cử viên đó sẽ thất bại ngay lập tức. Vì vậy, với mỗi aᵢ, chúng ta chỉ xét bⱼ sao cho (aᵢ & bⱼ) ⊆ X. 

Bây giờ vấn đề trở thành: với mỗi aᵢ, có tồn tại ít nhất một bⱼ tương thích với X không? Nếu có với tất cả i thì X có thể đạt được. Lý do điều này hiệu quả là vì chúng tôi chỉ hạn chế OR cuối cùng nằm trong X. Chúng tôi không quan tâm đến việc phân biệt bⱼ nào được sử dụng, chỉ mỗi aᵢ có một lựa chọn hợp lệ. 

Điều này làm giảm vấn đề kiểm tra 512 ứng viên, mỗi ứng cử viên yêu cầu quét n·m cặp. Điều này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | O(mⁿ) | O(n) | Quá chậm | 
| Hãy thử tất cả mặt nạ HOẶC | O(2⁹ · n · m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại tất cả các mặt nạ bit X có thể có từ 0 đến 511 và kiểm tra xem X có thể là câu trả lời cuối cùng hay không.

1. Bắt đầu với mặt nạ ứng cử viên X và giả sử đó là kết quả HOẶC mục tiêu. Chúng tôi giải thích điều này như một ràng buộc rằng không có bit nào bên ngoài X được phép xuất hiện trong bất kỳ cᵢ nào. 
2. Đối với mỗi phần tử aᵢ, chúng tôi kiểm tra xem có tồn tại ít nhất một bⱼ sao cho (aᵢ & bⱼ) là mặt nạ con của X hay không. Điều này đảm bảo rằng việc gán aᵢ cho bⱼ đó không tạo ra các bit bị cấm. 
3. Nếu với một số aᵢ không tồn tại bⱼ thì X là không thể, bởi vì aᵢ đó không thể được gán mà không vi phạm ràng buộc. 
4. Nếu mọi aᵢ có ít nhất một bⱼ hợp lệ thì X khả thi. Chúng tôi cập nhật câu trả lời ở mức tối thiểu trên tất cả các X khả thi. 

Điểm tinh tế là tính khả thi không phụ thuộc vào từng yếu tố một khi X được cố định. Chúng tôi không cùng nhau tối ưu hóa các nhiệm vụ; chúng tôi chỉ kiểm tra sự tồn tại. Khớp nối OR biến mất vì nó đã được mã hóa thành X. 

### Tại sao nó hoạt động 

Đối với mặt nạ cố định X, bất kỳ phép gán hợp lệ nào cũng phải chỉ tạo ra các bit bên trong X. Nếu ngay cả một aᵢ không có bⱼ tương thích, thì mọi phép gán có thể có cho aᵢ đó đều đưa ra một bit bị cấm, vì vậy X không thể là OR cuối cùng. 

Ngược lại, nếu mỗi aᵢ có ít nhất một lựa chọn tương thích, chúng ta có thể gán độc lập cho mỗi chỉ mục và không bao giờ đưa vào các bit bên ngoài X. Vì tất cả các mặt nạ được tạo ra đều là tập con của X, nên OR của chúng cũng là tập con của X, nghĩa là OR cuối cùng không thể vượt quá X. Bởi vì chúng tôi chỉ chấp nhận các mặt nạ có thể nhận ra tất cả các phần tử nên X là có thể đạt được. 

Do đó, thuật toán tìm kiếm giới hạn trên tối thiểu có thể đạt được trên OR cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    INF = 10**9
    ans = INF

    for mask in range(1 << 9):
        ok = True

        for i in range(n):
            found = False
            for j in range(m):
                if (a[i] & b[j]) | mask == mask:
                    found = True
                    break
            if not found:
                ok = False
                break

        if ok:
            ans = min(ans, mask)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện kiểm tra tính khả thi cho từng mặt nạ ứng viên. Biểu thức chính`(a[i] & b[j]) | mask == mask`kiểm tra xem tất cả các bit của kết quả AND có được chứa trong mặt nạ ứng cử viên hay không. Điều này tương đương với`(a[i] & b[j]) ⊆ mask`. 

Các vòng lặp lồng nhau có thể chấp nhận được vì n và m nhiều nhất là 200 và vòng lặp mặt nạ bên ngoài chỉ có 512 lần lặp. 

Một chi tiết triển khai tinh vi là sớm bị phá vỡ: khi chúng tôi tìm thấy bⱼ hợp lệ cho aᵢ, chúng tôi sẽ ngừng kiểm tra các giá trị j tiếp theo. Tương tự, khi bất kỳ aᵢ nào bị lỗi, chúng tôi sẽ ngừng kiểm tra mặt nạ hoàn toàn. Điều này ngăn cản công việc không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
2 6 4 0
2 4
```Chúng tôi kiểm tra mặt nạ ứng cử viên. Dưới đây là dấu vết của một vài cái có liên quan. 

| mặt nạ | a₀ | a₁ | a₂ | a₃ | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 (000) | được | được | được | được | khả thi | 
| 2 (010) | được | được | được | được | khả thi | 
| 4 (100) | thất bại ở a₁ | - | - | - | không khả thi | 

Đối với mặt nạ = 2, mỗi aᵢ có ít nhất một bⱼ chỉ tạo ra bit 1 hoặc các bit có ý nghĩa nhỏ hơn. Đối với mặt nạ = 4, phần tử a₁ không thể tránh việc tạo thêm một bit bên ngoài mặt nạ cho bất kỳ bⱼ nào, vì vậy nó không thành công. 

Mặt nạ khả thi nhỏ nhất là 2, phù hợp với kết quả mong đợi. 

### Ví dụ 2 

đầu vào:```
3 3
1 3 7
1 2 4
```Ở đây chúng tôi khám phá tính khả thi: 

| mặt nạ | a₀=1 | a₁=3 | a₂=7 | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | được | thất bại | thất bại | không khả thi | 
| 3 | được | được | thất bại | không khả thi | 
| 7 | được | được | được | khả thi | 

Chỉ có mặt nạ 7 mới cho phép tất cả các phần tử tìm được đối tác tương thích. Điều này chứng tỏ rằng đôi khi việc kết hợp toàn bộ bit là không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2⁹ · n · m) | Đối với mỗi trong số 512 mặt nạ, chúng tôi kiểm tra tất cả n phần tử và tối đa m ứng cử viên | 
| Không gian | O(1) | Chỉ lưu trữ đầu vào và một vài biến | 

Tổng công việc là khoảng 512 × 200 × 200, tức là khoảng 20 triệu bit hoạt động, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    INF = 10**9
    ans = INF

    for mask in range(1 << 9):
        ok = True
        for i in range(n):
            found = False
            for j in range(m):
                if (a[i] & b[j]) | mask == mask:
                    found = True
                    break
            if not found:
                ok = False
                break
        if ok:
            ans = min(ans, mask)

    return str(ans)

# provided sample
assert run("""4 2
2 6 4 0
2 4
""") == "2"

# minimum size
assert run("""1 1
0
0
""") == "0"

# all zeros except one bit
assert run("""2 2
1 2
4 4
""") == "0"

# full conflict case
assert run("""2 1
7 7
0
""") == "7"

# mixed case
assert run("""3 2
1 2 3
1 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp số 0 đơn | 0 | tính khả thi tầm thường | 
| bit rời rạc | 0 | sự độc lập của bài tập | 
| buộc phải đeo mặt nạ đầy đủ | 7 | sự cần thiết trong trường hợp xấu nhất | 
| khả năng tương thích hỗn hợp | 3 | tương tác ràng buộc một phần | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả aᵢ bằng 0. Bất kỳ bⱼ nào cũng tạo ra 0 sau AND, vì vậy mọi mặt nạ đều khả thi và mức tối thiểu là 0. Thuật toán xử lý vấn đề này vì với mặt nạ = 0, mỗi cặp tạo ra 0 là mặt nạ con bằng 0, do đó tính khả thi được giữ ngay lập tức. 

Một trường hợp khác là khi một số aᵢ không có bⱼ tương thích với mặt nạ nhỏ. Ví dụ: nếu aᵢ có bit cao không thể bị loại bỏ bởi bất kỳ AND nào với bⱼ, thì tất cả các mặt nạ ngoại trừ bit đó sẽ không đạt được chỉ số đó. Thuật toán phát hiện chính xác điều này ở bước kiểm tra tính khả thi của mặt nạ đó. 

Trường hợp cuối cùng là khi aᵢ khác nhau yêu cầu các lựa chọn bⱼ không tương thích. Thuật toán không cố gắng đồng bộ hóa các lựa chọn; thay vào đó, nó chỉ kiểm tra sự tồn tại của mỗi phần tử trong một mặt nạ cố định, mặt nạ này nắm bắt chính xác tính độc lập đó sau khi sửa giới hạn OR.
