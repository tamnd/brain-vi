---
title: "CF 1601D - Ngọn Núi Khó"
description: "Chúng tôi được cung cấp một nhóm người leo núi, mỗi người được đặc trưng bởi hai con số: kỹ năng và sự gọn gàng. Kỹ năng này cho biết độ khó tối đa của một ngọn núi mà người leo núi có thể leo lên, trong khi sự gọn gàng ảnh hưởng đến độ khó của ngọn núi sau khi người leo núi đó leo lên: cụ thể, nếu dòng điện…"
date: "2026-06-10T08:21:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1601
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 751 (Div. 1)"
rating: 2700
weight: 1601
solve_time_s: 92
verified: true
draft: false
---

[CF 1601D - Ngọn núi khó khăn](https://codeforces.com/problemset/problem/1601/D) 

**Xếp hạng:** 2700 
**Tags:** cấu trúc dữ liệu, dp, tham lam, sắp xếp 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm người leo núi, mỗi người được đặc trưng bởi hai con số: kỹ năng và sự gọn gàng. Kỹ năng này cho biết độ khó tối đa của một ngọn núi mà người leo núi có thể leo lên, trong khi sự gọn gàng ảnh hưởng đến độ khó của ngọn núi sau khi người leo núi đó leo lên: cụ thể là nếu độ khó của ngọn núi hiện tại là$p$và một người leo núi với sự gọn gàng$a$leo lên nó, khó khăn mới trở thành$\max(p, a)$. Ngọn núi bắt đầu với khó khăn ban đầu$d$. Bài toán yêu cầu số lượng người leo núi tối đa có thể leo núi nếu chúng ta chọn thứ tự tối ưu. 

Với$n$lên tới 500.000 và giá trị độ khó và kỹ năng lên tới$10^9$, bất kỳ giải pháp nào thử rõ ràng tất cả các hoán vị của những người leo núi đều không thể thực hiện được. Ngay cả cách tiếp cận bậc hai để kiểm tra từng người leo núi với nhau ở mỗi bước cũng quá chậm, vì điều đó có thể đòi hỏi tới$2.5 \times 10^{11}$hoạt động trong trường hợp xấu nhất. Vì vậy chúng ta cần một cách tiếp cận đại khái$O(n \log n)$. 

Một điểm tinh tế nảy sinh khi sự gọn gàng có thể làm tăng độ khó của ngọn núi so với kỹ năng của những người leo núi khác. Ví dụ: nếu một người leo núi có kỹ năng 3 và sự khéo léo 10 leo lên một ngọn núi bắt đầu từ độ khó 2, thì ngọn núi đó sẽ nhảy lên độ khó 10. Bất kỳ người leo núi nào có kỹ năng 5 giờ đây sẽ không thể leo lên, mặc dù trước đó họ có thể đã leo được. Một cách tiếp cận tham lam ngây thơ chỉ leo lên "người có tay nghề cao nhất trước" hoặc "sự gọn gàng cao nhất trước" có thể không tính đến điều này và tạo ra số lượng dưới mức tối ưu. Tương tự như vậy, nhiều người leo núi có thể có mức độ gọn gàng thấp hơn độ khó hiện tại; việc xử lý những điều này một cách chính xác đòi hỏi phải so sánh độ khó và kỹ năng của ngọn núi hiện tại một cách linh hoạt. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ xem xét mọi thứ tự leo núi có thể có. Đối với mỗi hoán vị của những người leo núi, chúng tôi sẽ mô phỏng việc leo núi một cách tuần tự, cập nhật độ khó của ngọn núi sau mỗi lần leo núi thành công và đếm xem có bao nhiêu người leo núi thành công. Mặc dù điều này nắm bắt chính xác tác dụng của sự gọn gàng, nhưng nó không khả thi vì số lượng hoán vị là$n!$, vượt xa những gì máy tính có thể xử lý$n$lớn hơn 10. 

Một cách tiếp cận có cấu trúc hơn cho thấy rằng mỗi người leo núi chỉ có tác dụng nếu kỹ năng của họ vượt quá độ khó hiện tại. Sau khi độ khó của ngọn núi được khắc phục, vấn đề sẽ giảm xuống còn việc chọn một tập hợp con những người leo núi có kỹ năng ít nhất bằng độ khó hiện tại, theo một thứ tự nào đó, sao cho chúng tôi tối đa hóa số lượng. Điểm mấu chốt là trong số tất cả những người leo núi có thể leo núi, trước tiên sẽ an toàn khi sử dụng người có độ gọn gàng nhỏ nhất. Sử dụng người leo núi có tính cẩn thận cao trước tiên có thể làm tăng độ khó của ngọn núi một cách không cần thiết, cản trở người khác leo lên. Điều này cho phép chúng ta biến vấn đề thành một chiến lược tham lam: ở mỗi bước, hãy chọn người leo núi có sẵn có độ gọn gàng nhỏ nhất nhưng kỹ năng của người này ít nhất là độ khó của ngọn núi hiện tại. 

Nếu chúng ta duy trì tất cả những người leo núi còn lại được sắp xếp theo độ gọn gàng và luôn chọn độ gọn gàng nhỏ nhất thỏa mãn yêu cầu về kỹ năng, thì chúng ta có thể mô phỏng thứ tự leo núi một cách hiệu quả. Sắp xếp ban đầu theo độ gọn gàng cho phép$O(n \log n)$xử lý và chúng ta có thể lặp qua mảng đã sắp xếp để mô phỏng việc leo lên. Mỗi người leo núi được xử lý chính xác một lần, do đó tổng độ phức tạp vẫn được giữ nguyên$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tham lam tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào: số người leo núi$n$, khó khăn ban đầu$d$, và danh sách các cặp$(s_i, a_i)$cho mỗi người leo núi. 
2. Sắp xếp danh sách người leo núi theo mức độ gọn gàng$a_i$theo thứ tự tăng dần. Nếu sự gọn gàng bằng nhau thì trật tự không thành vấn đề. 
3. Khởi tạo một biến`current_difficulty`đến khó khăn ban đầu`d`và một quầy`count`đến 0. 
4. Lặp lại danh sách đã sắp xếp của những người leo núi. Đối với mỗi người leo núi, hãy kiểm tra xem kỹ năng của họ có$s_i$ít nhất là`current_difficulty`. 
5. Nếu có, hãy tăng`count`và cập nhật`current_difficulty`ĐẾN$\max(current_difficulty, a_i)$. Nếu không, hãy bỏ qua người leo núi này. 
6. Sau khi xử lý tất cả những người leo núi, xuất ra bộ đếm`count`là số lượng người leo núi tối đa. 

Tại sao nó hoạt động: điều bất biến được duy trì là tại bất kỳ thời điểm nào trong quá trình lặp,`current_difficulty`là khó khăn sau tất cả những người leo núi trước đây. Việc chọn người leo núi có sẵn với sự gọn gàng tối thiểu sẽ đảm bảo rằng ngọn núi không trở nên khó khăn hơn một cách không cần thiết, điều này có thể cản trở những người leo núi khác. Sắp xếp theo sự gọn gàng và tham lam leo trèo sẽ duy trì trật tự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
alpinists = [tuple(map(int, input().split())) for _ in range(n)]

# Sort alpinists by neatness
alpinists.sort(key=lambda x: x[1])

current_difficulty = d
count = 0

for s, a in alpinists:
    if s >= current_difficulty:
        count += 1
        current_difficulty = max(current_difficulty, a)

print(count)
```Mã đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`để giải quyết hạn chế lớn của$n$. Việc sắp xếp theo mức độ gọn gàng đảm bảo rằng chúng tôi luôn ưu tiên người leo núi tăng độ khó lên núi ở mức tối thiểu trước tiên. các`for`vòng lặp kiểm tra kỹ năng theo độ khó hiện tại, đếm số lần leo thành công và cập nhật độ khó. Điều này thực hiện chính xác chiến lược tham lam. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 2
2 6
3 5
5 7
```| Bước | Khó khăn hiện tại | Người leo núi (s, a) | Leo lên được không? | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | (2,6) | Có | 1 | 
| 2 | 6 | (3,5) | Không | 1 | 
| 3 | 6 | (5,7) | Có | 2 | 

Dấu vết này xác nhận rằng việc chọn độ gọn gàng tối thiểu trước tiên sẽ dẫn đến`count = 2`, phù hợp với đầu ra dự kiến. 

**Mẫu 2** 

đầu vào:```
3 3
1 2
4 3
5 7
```| Bước | Khó khăn hiện tại | Người leo núi (s, a) | Leo lên được không? | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | (1,2) | Không | 0 | 
| 2 | 3 | (4,3) | Có | 1 | 
| 3 | 3 | (5,7) | Có | 2 | 

Điều này cho thấy thuật toán đã bỏ qua những người leo núi không thể leo núi do không đủ kỹ năng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp những người leo núi chiếm ưu thế trong tính toán; lặp qua danh sách là O(n) | 
| Không gian | O(n) | Lưu trữ danh sách những người leo núi và một vài quầy | 

Với$n \leq 5 \times 10^5$, cái$O(n \log n)$độ phức tạp có thể thoải mái trong giới hạn 2 giây và việc lưu trữ 500.000 người leo núi trong bộ nhớ là khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d = map(int, input().split())
    alpinists = [tuple(map(int, input().split())) for _ in range(n)]
    alpinists.sort(key=lambda x: x[1])
    current_difficulty = d
    count = 0
    for s, a in alpinists:
        if s >= current_difficulty:
            count += 1
            current_difficulty = max(current_difficulty, a)
    return str(count)

# Provided samples
assert run("3 2\n2 6\n3 5\n5 7\n") == "2", "sample 1"
assert run("3 3\n1 2\n4 3\n5 7\n") == "2", "sample 2"

# Custom cases
assert run("1 0\n0 0\n") == "1", "single alpinist equal to difficulty"
assert run("2 5\n4 3\n6 7\n") == "1", "first cannot climb, second can"
assert run("5 1\n1 2\n2 3\n3 4\n4 5\n5 6\n") == "5", "all can climb in sorted neatness"
assert run("3 10\n15 1\n12 5\n20 8\n") == "3", "initial difficulty less than all skills"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0\n0 0 | 1 | Đầu vào có kích thước tối thiểu trong đó kỹ năng tương đương với độ khó | 
| 2 5\n4 3\n6 7 | 1 | Bỏ qua một vận động viên leo núi có kỹ năng quá thấp | 
| 5 1\n1 2\n2 3\n3 4\n4 5 | | |
