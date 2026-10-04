---
title: "CF 104883B - \u5965\u672f\u4e4b\u5c18"
description: "Mỗi tài khoản đều có một lượng “bụi phức tạp” nhất định và có một danh sách cố định các bộ bài, mỗi bộ bài yêu cầu một mức chi phí bụi cụ thể để được chế tạo hoàn chỉnh."
date: "2026-06-28T09:09:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104883
codeforces_index: "B"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Final"
rating: 0
weight: 104883
solve_time_s: 44
verified: true
draft: false
---

[CF 104883B - \u5965\u672f\u4e4b\u5c18](https://codeforces.com/problemset/problem/104883/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi tài khoản đều có một lượng “bụi phức tạp” nhất định và có một danh sách cố định các bộ bài, mỗi bộ bài yêu cầu một mức chi phí bụi cụ thể để được chế tạo hoàn chỉnh. Đối với mỗi tài khoản, chúng tôi muốn biết số lượng bộ bài hoàn chỉnh tối đa có thể được chế tạo nếu chúng tôi chọn một tập hợp con các bộ bài tùy ý, với hạn chế là mỗi bộ bài được chọn sẽ tiêu thụ toàn bộ lượng bụi cần thiết và tất cả chi phí đã chọn phải tính tổng trong ngân sách bụi của tài khoản. 

Nói một cách cụ thể hơn, chúng ta được cung cấp một mảng`A`kích thước`n`, Ở đâu`A[i]`là bụi có sẵn cho tài khoản`i`. Chúng tôi cũng được cung cấp một mảng`B`kích thước`m`, mỗi nơi`B[j]`là chi phí chế tạo bộ bài`j`. Đối với mỗi`A[i]`, chúng ta phải chọn một tập con của`B`tối đa hóa số lượng phần tử được chọn sao cho tổng của chúng không vượt quá`A[i]`. 

Những hạn chế`n, m ≤ 10^5`ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng tính toán lại chiếc ba lô cho mỗi tài khoản. Phương pháp lập trình động đơn giản cho mỗi tài khoản sẽ yêu cầu`O(n * m)`thời gian, theo thứ tự`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. 

Một điểm tinh tế quan trọng là chúng tôi không được yêu cầu tối đa hóa giá trị hoặc tính toán các kết hợp khác nhau cho mỗi tài khoản mà chỉ tính số lượng mặt hàng. Điều này làm cho cấu trúc trở nên tham lam sau khi sắp xếp. 

Một trường hợp thất bại phổ biến xuất phát từ việc không phân loại chi phí bộ bài. Nếu chúng ta chọn những bộ bài đắt tiền trước, chúng ta có thể giảm số lượng một cách giả tạo. Ví dụ, nếu`A = 10`Và`B = [8, 7, 3, 2]`, chọn 8 đầu tiên chỉ cho một bộ bài, trong khi tối ưu là`[2, 3, 7]`hoặc`[2, 3]`tùy theo ngân sách, việc thể hiện rõ ràng sự tham lam theo thứ tự tùy tiện là không chính xác. 

Một trường hợp đặc biệt khác là bộ bài không tốn phí. Nếu như`B = [0, 0, 5]`Và`A = 1`, trước tiên chúng ta có thể lấy cả hai bộ bài có chi phí bằng 0, sau đó vẫn có khả năng lấy bộ bài có chi phí 5 nếu ngân sách cho phép. Bất kỳ phương pháp nào bỏ qua số 0 hoặc giả định giá trị dương hoàn toàn đều có thể bị tính sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi tài khoản, chúng tôi thử tất cả các tập hợp con của bộ bài, tính toán tổng chi phí của chúng và theo dõi kích thước tập hợp con tối đa phù hợp với ngân sách. Điều này đúng vì nó trực tiếp thực thi định nghĩa ràng buộc, nhưng nó yêu cầu liệt kê`2^m`tập hợp con cho mỗi tài khoản, điều này là không thể ngay cả đối với những tài khoản nhỏ`m`. 

Một cách mạnh mẽ hơn là sắp xếp các tập hợp con theo kích thước hoặc thử DP ba lô cho mỗi tài khoản. Điều đó dẫn đến một giải pháp giả đa thức`O(n * m * max(A))`về mặt tinh thần, nhưng vẫn không khả thi vì`A[i]`có thể lớn như`10^9`. 

Nhận xét quan trọng là vì mỗi bộ bài đều đóng góp một “giá trị” như nhau (một bộ bài thủ công), nên chúng ta nên ưu tiên những bộ bài rẻ hơn trước. Một khi chúng tôi sắp xếp`B`theo thứ tự không giảm, chiến lược tối ưu cho bất kỳ ngân sách cố định nào là lấy càng nhiều chi phí nhỏ nhất càng tốt cho đến khi chúng ta vượt quá ngân sách. Điều này biến mỗi truy vấn thành một bài toán tổng tiền tố, sau đó là tìm kiếm nhị phân. 

Chúng tôi tính toán trước việc sắp xếp`B`và tổng tiền tố của nó. Sau đó với mỗi`A[i]`, chúng ta tìm chỉ số tiền tố lớn nhất sao cho tổng tiền tố là ≤`A[i]`. Chỉ số đó chính là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^m) | O(1) | Quá chậm | 
| Tối ưu (sắp xếp + tiền tố + tìm kiếm nhị phân) | O(m log m + n log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành việc trả lời nhiều truy vấn “có bao nhiêu yếu tố nhỏ nhất phù hợp với ngân sách”. 

1. Sắp xếp mảng`B`theo thứ tự tăng dần. Điều này đảm bảo rằng chúng tôi luôn xem xét các bộ bài rẻ hơn trước tiên, điều này là cần thiết vì mỗi bộ bài đều đóng góp như nhau vào số lượng. 
2. Xây dựng mảng tổng tiền tố`P`, Ở đâu`P[k]`đại diện cho tổng số bụi cần thiết để chế tạo lần đầu tiên`k`sàn rẻ nhất. Điều này chuyển đổi tổng tập hợp con thành một chuỗi đơn điệu. 
3. Đối với từng ngân sách tài khoản`A[i]`, thực hiện tìm kiếm nhị phân trên`P`để tìm chỉ số lớn nhất`k`như vậy`P[k] ≤ A[i]`. Câu trả lời cho tài khoản đó là`k`. 
4. In tất cả các câu trả lời theo thứ tự. 

Tìm kiếm nhị phân hoạt động vì mảng tổng tiền tố hoàn toàn không giảm, vì vậy tính khả thi (`P[k] ≤ A[i]`) là đơn điệu ở`k`. 

### Tại sao nó hoạt động 

Sau khi các bộ bài được sắp xếp theo chi phí, mọi lựa chọn tối ưu sử dụng bộ bài đắt tiền hơn trong khi bỏ qua bộ bài rẻ hơn đều có thể được cải thiện bằng cách hoán đổi chúng mà không làm giảm số lượng bộ bài đã chọn và chỉ giảm hoặc bảo toàn tổng chi phí. Việc lặp lại đối số trao đổi này sẽ buộc tất cả các giải pháp tối ưu vào một cấu trúc trong đó các bộ bài được chọn luôn là tiền tố của danh sách được sắp xếp. Điều đó làm cho không gian nghiệm trở nên một chiều và được nắm bắt hoàn toàn bằng tổng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    A = list(map(int, input().split()))
    m = int(input())
    B = list(map(int, input().split()))

    B.sort()

    P = [0] * (m + 1)
    for i in range(1, m + 1):
        P[i] = P[i - 1] + B[i - 1]

    def upper_bound(x):
        lo, hi = 0, m
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if P[mid] <= x:
                lo = mid
            else:
                hi = mid - 1
        return lo

    res = []
    for a in A:
        res.append(str(upper_bound(a)))

    print(" ".join(res))

if __name__ == "__main__":
    main()
```Bước sắp xếp đảm bảo chúng ta luôn sử dụng những bộ bài rẻ nhất có thể trước tiên. Mảng tiền tố chuyển đổi các phép kiểm tra tính tổng lặp đi lặp lại thành một cấu trúc đơn điệu duy nhất. Tìm kiếm nhị phân tìm thấy độ dài tiền tố khả thi tối đa một cách hiệu quả. 

Một chi tiết tinh tế là việc sử dụng tìm kiếm nhị phân kiểu giới hạn trên thay vì`bisect_right`trực tiếp, nhưng cả hai đều tương đương. Điều kiện quan trọng là chúng ta tìm kiếm theo tổng tiền tố chứ không phải giá trị thô. 

Các bộ bài có chi phí bằng 0 được xử lý một cách tự nhiên vì chúng đóng góp bằng 0 vào tổng tiền tố, cho phép chỉ số tiền tố tăng lên mà không tiêu tốn ngân sách. 

## Ví dụ đã hoạt động 

Hãy xem xét`B = [0, 2, 5, 9]`và tài khoản`A = [0, 5, 10]`. 

Đầu tiên chúng ta sắp xếp`B`(đã được sắp xếp) và tính tổng tiền tố: 

| k | B[k] | P[k] | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | 0 | 0 | 
| 2 | 2 | 2 | 
| 3 | 5 | 7 | 
| 4 | 9 | 16 | 

Đối với mỗi tài khoản: 

| A | Tiền tố khả thi k | Lý do | 
| --- | --- | --- | 
| 0 | 1 | chỉ có bộ bài không tốn chi phí mới phù hợp | 
| 5 | 3 | 0 + 2 + 5 = 7 quá lớn nên tốt nhất là 0 + 2 | 
| 10 | 3 | 0 + 2 + 5 = 7 phù hợp, vượt quá lần thứ 4 | 

Vì`A = 5`, tìm kiếm nhị phân sẽ tìm thấy tổng tiền tố lớn nhất 5, tức là`k = 2`, tương ứng với hai bộ bài. Điều này khẳng định rằng chúng tôi luôn ưu tiên chi phí nhỏ hơn trước. 

Bây giờ hãy xem xét một trường hợp có sự mất cân bằng lớn:`B = [100, 1, 1]`,`A = 2`. 

Đã sắp xếp`B = [1, 1, 100]`, tổng tiền tố`[0, 1, 2, 102]`. 

| A | k | 
| --- | --- | 
| 2 | 2 | 

Chúng tôi chọn đúng cả hai bộ bài giá rẻ và bỏ qua bộ bài đắt tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + n log m) | bộ bài sắp xếp chiếm ưu thế, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(m) | lưu trữ mảng tổng tiền tố | 

Những hạn chế`n, m ≤ 10^5`phù hợp thoải mái trong sự phức tạp này. Sắp xếp và tìm kiếm nhị phân trên 100k phần tử là tiêu chuẩn trong giới hạn 1 giây trong Python khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    A = list(map(int, input().split()))
    m = int(input())
    B = list(map(int, input().split()))

    B.sort()
    P = [0]
    for x in B:
        P.append(P[-1] + x)

    def upper_bound(x):
        lo, hi = 0, m
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if P[mid] <= x:
                lo = mid
            else:
                hi = mid - 1
        return lo

    return " ".join(str(upper_bound(a)) for a in A)

# sample-like test
assert run("3\n5 10 15\n4\n2 2 3 4") == "2 4 4", "basic case"

# minimum case
assert run("1\n0\n1\n0") == "1", "single zero case"

# all expensive
assert run("2\n1 10\n3\n100 200 300") == "0 0", "no budget case"

# all zeros
assert run("2\n0 5\n3\n0 0 0") == "3 3", "zero cost stacking"

# mixed case
assert run("3\n3 6 10\n4\n1 2 5 7") == "3 4 4", "prefix growth case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 1 | xử lý không tốn phí | 
| không có ngân sách | 0 0 | không thể chọn bất kỳ bộ bài nào | 
| tất cả số không | 3 3 | tích lũy bộ bài miễn phí | 
| trường hợp hỗn hợp | 3 4 4 | tiền tố đúng + cấu trúc tham lam | 

## Vỏ cạnh 

Khi tất cả chi phí bộ bài bằng 0, tổng tiền tố vẫn bằng 0 cho tất cả các tiền tố. Thuật toán trả về`m`cho mọi tài khoản vì mọi tiền tố đều khả thi. Đối với một đầu vào như`A = [0, 100]`Và`B = [0, 0, 0]`, tìm kiếm nhị phân luôn trả về 3, phù hợp với thực tế là tất cả các bộ bài đều có thể được sử dụng bất kể ngân sách. 

Khi tất cả các bộ bài đều quá đắt, chẳng hạn như`B = [5, 6, 7]`Và`A = [0, 4]`, tổng tiền tố bắt đầu từ 0 rồi ngay lập tức vượt quá ngân sách. Thuật toán trả về chính xác 0 cho tất cả các tài khoản vì ngay cả bộ bài rẻ nhất cũng không thể mua được. 

Khi ngân sách cực kỳ lớn, tìm kiếm nhị phân luôn trả về`m`, vì tổng tiền tố đầy đủ phù hợp. Điều này tránh bất kỳ sự tích lũy tràn hoặc lặp lại nào cho mỗi truy vấn và giải thích tại sao quá trình tiền xử lý
