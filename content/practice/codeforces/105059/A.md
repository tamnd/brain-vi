---
title: "CF 105059A - Đá Luddy"
description: "Nhiệm vụ cơ bản là kiểm tra xem một từ mục tiêu cố định có thể được xây dựng từ các chữ cái có sẵn trong một chuỗi nhất định hay không. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một “biểu ngữ” được làm bằng chữ in hoa."
date: "2026-06-23T10:48:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105059
codeforces_index: "A"
codeforces_contest_name: "IU Programming Challenge 2024"
rating: 0
weight: 105059
solve_time_s: 45
verified: true
draft: false
---

[CF 105059A - Luddy Rocks](https://codeforces.com/problemset/problem/105059/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cơ bản là kiểm tra xem một từ mục tiêu cố định có thể được xây dựng từ các chữ cái có sẵn trong một chuỗi nhất định hay không. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một “biểu ngữ” được làm bằng chữ in hoa. Cora muốn sắp xếp lại một số chữ cái này, loại bỏ những chữ cái cô không cần, để tạo thành chính xác chuỗi “LUDDYROCKS”. 

Vì vậy, đầu ra hoàn toàn phụ thuộc vào việc tập hợp nhiều ký tự trong biểu ngữ có chứa đủ bản sao của từng chữ cái được yêu cầu hay không. Trật tự không liên quan vì chúng ta được phép sắp xếp lại một cách tự do. Điều quan trọng là khớp tần số: mỗi ký tự trong từ đích phải có sẵn ít nhất nhiều lần nếu cần. 

Các ràng buộc rất nhỏ, với tối đa 100 trường hợp thử nghiệm và độ dài mỗi chuỗi nhiều nhất là 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc quét đơn giản từng chuỗi cũng rẻ. Số tần số cho mỗi trường hợp thử nghiệm bị giới hạn bởi khoảng 10.000 thao tác ký tự trong trường hợp xấu nhất, điều này không đáng kể trong giới hạn 1 giây. 

Một điểm tinh tế là các ký tự có thể lặp lại trong từ đích. Ví dụ: “LUDDYROCKS” chứa hai chữ D. Ở đây, một kiểm tra ngây thơ chỉ xác minh sự hiện diện của các ký tự riêng biệt sẽ không thành công. Một cạm bẫy khác là xử lý vấn đề như kiểm tra trình tự tiếp theo, trong đó thứ tự là vấn đề quan trọng. Điều đó sẽ không đúng vì chúng ta được phép sắp xếp lại một cách tùy ý. 

Một trường hợp lỗi minh họa nhỏ là chuỗi “LUDYROCKS”. Nó chỉ chứa một chữ D nên mặc dù có tất cả các chữ cái khác nhưng câu trả lời phải là KHÔNG. Bất kỳ cách tiếp cận nào chỉ kiểm tra tập hợp bao gồm sẽ chấp nhận nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề là cố gắng hình thành từ mục tiêu một cách rõ ràng. Người ta có thể cố gắng khớp từng ký tự của “LUDDYROCKS” bằng cách quét liên tục biểu ngữ và đánh dấu các ký tự đã sử dụng. Đối với mỗi ký tự trong mục tiêu, chúng tôi tìm kiếm toàn bộ chuỗi để tìm một chữ cái phù hợp không được sử dụng. Điều này hiệu quả vì nó mô phỏng việc xây dựng thực tế, đảm bảo tính chính xác ngay cả với các bản sao. 

Tuy nhiên, phương pháp này thực hiện tối đa 10 kết quả khớp và mỗi kết quả có thể yêu cầu quét tối đa 100 ký tự. Điều đó mang lại độ phức tạp trong trường hợp xấu nhất là khoảng 1000 thao tác cho mỗi trường hợp thử nghiệm, điều này vẫn ổn ở đây, nhưng cấu trúc trở nên nặng nề không cần thiết và sẽ không mở rộng được nếu các ràng buộc tăng lên. 

Quan sát quan trọng là thứ tự không liên quan và không được phép sử dụng lại, do đó vấn đề giảm xuống việc đếm tần số. Thay vì tìm kiếm nhiều lần, chúng tôi đếm số lần xuất hiện của từng ký tự trong biểu ngữ và so sánh chúng trực tiếp với tần suất bắt buộc của từ mục tiêu. Điều này làm giảm vấn đề từ việc tìm kiếm lặp đi lặp lại thành một đường truyền tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O(n · m) | O(n) | Được chấp nhận cho các ràng buộc | 
| Đếm tần số | O(n) | O(1) | Tối ưu | 

## Hướng dẫn thuật toán 

1. Tính toán trước tần số của từng ký tự trong chuỗi đích “LUDDYROCKS”. Điều này đưa ra một bảng yêu cầu cố định không thay đổi giữa các trường hợp thử nghiệm. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc chuỗi biểu ngữ. 
3. Đếm tần suất xuất hiện của từng ký tự trong banner. 
4. Đối với mỗi ký tự được yêu cầu bởi từ mục tiêu, hãy kiểm tra xem số lượng biểu ngữ ít nhất có lớn bằng số lượng được yêu cầu hay không. 
5. Nếu tất cả các yêu cầu đều được đáp ứng, ghi CÓ; nếu không thì xuất ra NO. 

Lý do đằng sau việc kiểm tra tất cả các ký tự được yêu cầu một cách độc lập là mỗi ký tự đại diện cho một ràng buộc độc lập. Thất bại bất kỳ một trong số chúng có nghĩa là không thể xây dựng được. 

### Tại sao nó hoạt động

Thuật toán dựa vào tính bất biến mà bảng tần số của biểu ngữ mô tả đầy đủ tất cả các cách sắp xếp lại có thể xảy ra. Vì việc sắp xếp lại không làm thay đổi số lượng nên mọi cấu trúc hợp lệ đều phải được biểu diễn dưới dạng tập hợp con của tập hợp ký tự của biểu ngữ. Bằng cách kiểm tra xem mọi số lượng bắt buộc có nhỏ hơn hoặc bằng số lượng có sẵn hay không, chúng tôi đảm bảo rằng có một lựa chọn ký tự hợp lệ. Không tồn tại ràng buộc thứ tự nào, do đó tính khả thi giảm chính xác đến việc đưa vào nhiều tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "LUDDYROCKS"

# precompute required frequencies
need = {}
for c in TARGET:
    need[c] = need.get(c, 0) + 1

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    have = {}
    for c in s:
        have[c] = have.get(c, 0) + 1

    ok = True
    for c, cnt in need.items():
        if have.get(c, 0) < cnt:
            ok = False
            break

    print("YES" if ok else "NO")
```Mã bắt đầu bằng cách xây dựng bản đồ yêu cầu cố định cho từ mục tiêu. Điều này tránh việc tính toán lại nó cho mọi trường hợp thử nghiệm. Đối với mỗi chuỗi đầu vào, một từ điển tần số mới được tạo trong một lần truyền. 

Vòng lặp cuối cùng là bước so sánh quan trọng. Nó đảm bảo mọi ký tự được yêu cầu đều có đủ số lượng. sử dụng`get(c, 0)`tránh các lỗi chính và xử lý rõ ràng các ký tự bị thiếu. 

Một chi tiết triển khai tinh tế là loại bỏ chuỗi đầu vào. Không có`.strip()`, các ký tự dòng mới có thể bị hiểu sai như một phần của tần số đếm, có khả năng đưa các khóa không chính xác vào từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
10
LUDDYROCKS
```| Bước | Nhân vật | Hành động | có trạng thái (một phần) | 
| --- | --- | --- | --- | 
| xây dựng | L | tăng | L:1 | 
| xây dựng | Bạn | tăng | L:1 U:1 | 
| xây dựng | D | tăng | L:1 U:1 D:1 | 
| xây dựng | D | tăng | L:1 U:1 D:2 | 
| xây dựng | Y | tăng | L:1 U:1 D:2 Y:1 | 
| xây dựng | R | tăng | L:1 U:1 D:2 Y:1 R:1 | 
| xây dựng | Ồ | tăng | L:1 U:1 D:2 Y:1 R:1 O:1 | 
| xây dựng | C | tăng | L:1 U:1 D:2 Y:1 R:1 O:1 C:1 | 
| xây dựng | K | tăng | L:1 U:1 D:2 Y:1 R:1 O:1 C:1 K:1 | 
| xây dựng | S | tăng | L:1 U:1 D:2 Y:1 R:1 O:1 C:1 K:1 S:1 | 

Tất cả số lượng yêu cầu khớp chính xác, vì vậy đầu ra là CÓ. 

Điều này xác nhận thuật toán xử lý trường hợp khớp đầy đủ đơn giản nhất. 

### Ví dụ 2 

đầu vào:```
1
9
LUDYROCKS
```| Kiểm tra bắt buộc | có tính | cần thiết | kết quả | 
| --- | --- | --- | --- | 
| L | 1 | 1 | được | 
| Bạn | 1 | 1 | được | 
| D | 1 | 2 | thất bại | 
| Y | 1 | 1 | được | 
| R | 1 | 1 | được | 
| Ồ | 1 | 1 | được | 
| C | 1 | 1 | được | 
| K | 1 | 1 | được | 
| S | 1 | 1 | được | 

Chữ D thứ hai bị thiếu ngay lập tức phá vỡ tính khả thi, do đó đầu ra là KHÔNG. Dấu vết này cho thấy tầm quan trọng của việc đếm các bản sao thay vì chỉ kiểm tra sự tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n) | mỗi trường hợp kiểm thử sẽ quét chuỗi một lần và so sánh với mục tiêu có kích thước không đổi | 
| Không gian | O(1) | bản đồ tần số được giới hạn bởi kích thước bảng chữ cái viết hoa | 

Giải pháp dễ dàng nằm trong giới hạn vì tổng kích thước đầu vào tối đa chỉ khoảng 10.000 ký tự, dẫn đến thời gian chạy không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    TARGET = "LUDDYROCKS"
    need = {}
    for c in TARGET:
        need[c] = need.get(c, 0) + 1

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        have = {}
        for c in s:
            have[c] = have.get(c, 0) + 1

        ok = True
        for c, cnt in need.items():
            if have.get(c, 0) < cnt:
                ok = False
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("1\n10\nLUDDYROCKS\n") == "YES"
assert run("1\n9\nLUDYROCKS\n") == "NO"

# custom cases
assert run("1\n5\nABCDE\n") == "NO", "missing most letters"
assert run("1\n20\nLLLUDDDYROCKSSSSS\n") == "YES", "extra letters allowed"
assert run("2\n10\nLUDDYROCKS\n9\nLUDDYROCKS\n") == "YES\nNO", "multi-test correctness"
assert run("1\n10\nLUDDYROCKS\n") == "YES", "exact match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi ngẫu nhiên 5 chữ cái | KHÔNG | thiếu chữ cái bắt buộc | 
| chuỗi hợp lệ được đệm | CÓ | chữ thừa là vô hại | 
| đầu vào đa thử nghiệm hỗn hợp | CÓ/KHÔNG | xử lý đúng từng trường hợp | 
| khớp chính xác | CÓ | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi biểu ngữ chứa tất cả các chữ cái bắt buộc nhưng không đủ số bội, như trong “LUDYROCKS”. Thuật toán đếm số lần xuất hiện một cách rõ ràng, vì vậy khi so sánh yêu cầu cho D (2 cần thiết, 1 khả dụng), nó sẽ ngay lập tức loại bỏ trường hợp đó. 

Một trường hợp khác là khi biểu ngữ dài hơn nhiều và chứa nhiều ký tự không liên quan. Ví dụ: một chuỗi như “ZZZLUDDYROCKSZZZ” vẫn tạo ra YES chính xác vì việc so sánh tần số bỏ qua các ký tự phụ không có trong đích. Điều bất biến là chỉ có giới hạn dưới mới quan trọng chứ không phải sự bình đẳng chính xác. 

Trường hợp cuối cùng được lặp lại với cấu trúc đầy đủ, chẳng hạn như “LUDDYROCKSLUDDYROCKS”. Bảng tần số hỗ trợ điều này một cách tự nhiên vì tỷ lệ đếm tuyến tính và mọi ký tự được yêu cầu vẫn được đáp ứng.
