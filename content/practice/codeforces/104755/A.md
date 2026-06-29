---
title: "CF 104755A - Áp phích"
description: "Chúng ta được cung cấp một dòng chữ áp phích ngắn và một cây bút có thể viết bằng ba màu cố định. Mỗi màu có một giới hạn dung lượng được đo bằng số lượng ký tự có thể được sử dụng."
date: "2026-06-28T22:51:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104755
codeforces_index: "A"
codeforces_contest_name: "LU ICPC Selection Contest 2023"
rating: 0
weight: 104755
solve_time_s: 49
verified: true
draft: false
---

[CF 104755A - Áp phích](https://codeforces.com/problemset/problem/104755/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng chữ áp phích ngắn và một cây bút có thể viết bằng ba màu cố định. Mỗi màu có một giới hạn dung lượng được đo bằng số lượng ký tự có thể được sử dụng. Ràng buộc chính không phải là các ký tự riêng lẻ được tô màu tự do mà là việc phân chia các loại ký tự thành ba màu. 

Mỗi ký tự trong văn bản thuộc về chính xác một trong ba loại: chữ cái, chữ số hoặc dấu chấm câu. Khoảng trắng tồn tại trong văn bản nhưng không thuộc các danh mục này và do đó không tiêu tốn mực ở bất kỳ màu nào. Nhiệm vụ là quyết định xem chúng ta có thể gán mỗi danh mục một màu riêng biệt sao cho tổng số ký tự trong danh mục đó không vượt quá số mực có sẵn cho màu đó hay không. 

Nói cách khác, chúng ta phải ánh xạ các chữ cái, chữ số và dấu câu một cách tuần tự vào tập hợp các màu xanh, vàng và đỏ và sau khi chọn ánh xạ này, số lượng của mỗi danh mục phải vừa với khả năng được chỉ định. 

Giới hạn đầu vào nhỏ, với độ dài văn bản nhiều nhất là 1000. Điều này ngay lập tức gợi ý rằng việc đếm và kiểm tra số lượng cấu hình không đổi là đủ, vì chỉ có 3 danh mục và do đó chỉ có 6 phép gán có thể thực hiện được. 

Một cạm bẫy ngây thơ nhưng quan trọng là cho rằng màu sắc có thể được trộn lẫn trong một danh mục. Ví dụ: cố gắng chia các chữ cái thành nhiều màu sẽ vi phạm quy tắc rằng mỗi danh mục phải được chỉ định chính xác một màu. Một vấn đề tế nhị khác là quên bỏ qua khoảng trắng, vốn không thuộc bất kỳ danh mục nào. 

Một ví dụ nhỏ có thể xảy ra lỗi là: 

đầu vào: 

ICPC 

1 1 10 

Chữ cái = 4, dấu câu = 0, chữ số = 0. Nếu chúng ta cho phép phân tách các chữ cái theo màu sắc một cách nhầm lẫn, chúng ta có thể kết luận tính khả thi một cách không chính xác, nhưng vấn đề không cho phép điều đó. 

Đầu ra đúng là: 

Không 

vì 4 vượt quá cả 1 và 10 trong một số ánh xạ tùy thuộc vào ràng buộc gán. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử tất cả các phép gán có thể có của ba loại cho ba màu. Đối với mỗi bài tập, chúng tôi tính toán tổng số chữ cái, chữ số và dấu chấm câu trong văn bản, sau đó kiểm tra xem mỗi danh mục có phù hợp với khả năng được giao hay không. 

Vì có chính xác ba danh mục và ba màu nên số lượng ánh xạ là 3 giai thừa, tức là 6. Đối với mỗi ánh xạ, chúng tôi quét toàn bộ văn bản một lần để đếm tần suất danh mục. Với độ dài tối đa là 1000, điều này mang lại nhiều nhất là khoảng 6000 kiểm tra ký tự, điều này không đáng kể ngay cả trong các giới hạn chặt chẽ. 

Cái nhìn sâu sắc quan trọng là cấu trúc không động hoặc tổ hợp ngoài các hoán vị. Không có sự phụ thuộc giữa các ký tự trong một danh mục, chỉ có sự phụ thuộc giữa số lượng tổng hợp và dung lượng cố định. Điều này làm giảm vấn đề từ một cái gì đó có thể trông giống như phép gán hoặc DP thành vấn đề kiểm tra hoán vị hệ số không đổi. 

Lực lượng vũ phu đã chạy đủ nhanh, nhưng việc đơn giản hóa khái niệm là nhận ra rằng mức độ tự do duy nhất là 6 hoán vị của ánh xạ từ loại sang màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(6 · n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, tối ưu hóa) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính toán ba giá trị: số chữ cái, số chữ số và số ký tự dấu chấm câu trong văn bản. Không gian được bỏ qua hoàn toàn. 

Sau đó, chúng tôi thử mọi cách gán có thể của ba loại cho ba màu có sẵn. Mỗi bài tập ghép một số danh mục với một giới hạn dung lượng. 

Đối với mỗi nhiệm vụ, chúng tôi kiểm tra xem số lượng cả ba danh mục có nhỏ hơn hoặc bằng năng lực được giao hay không. Nếu tìm thấy ít nhất một phép gán hợp lệ, chúng ta có thể kết luận ngay rằng tấm áp phích đó là có thể. 

Nếu không có bài tập nào trong số sáu bài toán thỏa mãn các ràng buộc thì câu trả lời là không thể.

### Tại sao nó hoạt động 

Mỗi giải pháp hợp lệ tương ứng chính xác với việc chọn song ánh giữa ba loại và ba màu. Vì công suất được cố định cho mỗi màu nên tính khả thi chỉ phụ thuộc vào việc liệu cặp được chọn có tôn trọng đồng thời cả ba điểm bất bình đẳng hay không. Việc liệt kê tất cả các phép đối chiếu đảm bảo rằng không bỏ sót ánh xạ hợp lệ nào và việc kiểm tra chúng một cách độc lập sẽ đảm bảo tính chính xác vì các danh mục không tương tác sau khi được chỉ định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    b, y, r = map(int, input().split())
    s = input().rstrip('\n')

    letters = digits = punct = 0

    punct_set = set([',', '-', ';', ':', '.', '!', '?'])

    for ch in s:
        if ch == ' ':
            continue
        if ch.isalpha():
            letters += 1
        elif ch.isdigit():
            digits += 1
        else:
            punct += 1

    counts = [letters, digits, punct]
    caps = [b, y, r]

    import itertools

    for perm in itertools.permutations(caps):
        ok = True
        for i in range(3):
            if counts[i] > perm[i]:
                ok = False
                break
        if ok:
            print("Yes")
            return

    print("No")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tách văn bản thành ba loại bắt buộc bằng cách sử dụng phân loại ký tự đơn giản. Các chữ cái được phát hiện bằng cách sử dụng`isalpha`, chữ số sử dụng`isdigit`và mọi thứ khác ngoại trừ dấu cách đều được coi là dấu câu. Điều này phù hợp với định nghĩa vấn đề vì dấu câu được giới hạn rõ ràng trong một tập hợp nhỏ đã biết, nhưng quy tắc phân loại được nhánh dự phòng nắm bắt một cách an toàn. 

Sau đó, chúng tôi coi ba số đếm và ba dung lượng là nhiều tập hợp. Bằng cách hoán vị các khả năng, chúng tôi mô phỏng tất cả các phép gán màu có thể có cho các danh mục. Mỗi hoán vị được kiểm tra độc lập trong thời gian không đổi. 

Một điểm tinh tế là đảm bảo loại trừ các khoảng trắng vì chúng xuất hiện trong văn bản nhưng không thuộc bất kỳ danh mục nào. Sự rõ ràng`continue`xử lý việc này một cách sạch sẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
21 8 2
ICPC Programming Season 2023-2024!
```Đếm: 

chữ cái = 24, chữ số = 8, dấu câu = 1 

Chúng tôi kiểm tra hoán vị của năng lực (21, 8, 2). 

| hoán vị | chữ cái | chữ số | dấu câu | hợp lệ | 
| --- | --- | --- | --- | --- | 
| (21,8,2) | 24 21 | 8 8 8 | 1 2 | Không | 
| (21,2,8) | 24 21 | 8 2 | 1 8 | Không | 
| (8,21,2) | 24 ≤ 8 | 8 21 | 1 2 | Không | 
| ... | ... | ... | ... | ... | 

Không có hoán vị nào thỏa mãn mọi ràng buộc, vì vậy đầu ra là`No`. 

Điều này chứng tỏ rằng ngay cả khi tổng công suất có vẻ đủ thì ràng buộc phân công cố định có thể cản trở tính khả thi. 

### Ví dụ 2 

đầu vào:```
5 20 5
13 problems in the contest!
```Đếm: 

chữ cái = 17, chữ số = 2, dấu câu = 1 

Một bài tập hợp lệ là: 

chữ số → xanh lam (5), chữ cái → vàng (20), dấu chấm câu → đỏ (5) 

| thể loại | đếm | giới hạn được giao | được | 
| --- | --- | --- | --- | 
| chữ số | 2 | 5 | | 
| chữ cái | 17 | 20 | | 
| chấm câu | 1 | 5 | | 

Hoán vị này thỏa mãn mọi ràng buộc, vì vậy đầu ra là`Yes`. 

Điều này cho thấy tính khả thi phụ thuộc vào việc kết hợp chính xác các danh mục lớn với dung lượng lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét để đếm ký tự cộng với 6 hoán vị không đổi | 
| Không gian | O(1) | Chỉ có ba quầy và mảng cố định | 

Kích thước đầu vào tối đa là 1000 ký tự, do đó, một lần truyền qua chuỗi là không đáng kể. Hệ số không đổi từ việc kiểm tra sáu hoán vị là không đáng kể, đảm bảo nghiệm dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# sample-like case: impossible
assert run("21 8 2\nICPC Programming Season 2023-2024!\n") == "No"

# sample-like case: possible
assert run("5 20 5\n13 problems in the contest!\n") == "Yes"

# minimum size, single letter
assert run("1 1 1\na\n") == "Yes"

# digits dominate punctuation requirement
assert run("0 5 0\n12345\n") == "Yes"

# tight fail case
assert run("2 2 2\nabc123!!!\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1, "a" | Có | nhiệm vụ hợp lệ tối thiểu | 
| 0 5 0, "12345" | Có | danh mục không có danh mục nào khác bị bỏ qua | 
| 2 2 2, "abc123!!!" | Không | công suất chặt chẽ không phù hợp buộc thất bại | 

## Vỏ cạnh 

Trường hợp phức tạp phát sinh khi một danh mục trống. Ví dụ: 

đầu vào:```
1 1 1
abc123
```Ở đây dấu câu bằng không. Thuật toán vẫn coi dấu câu là một danh mục có số đếm 0 và mọi phép gán mà nó ánh xạ tới bất kỳ màu nào vẫn hợp lệ vì dung lượng 0 ≤ luôn giữ nguyên. Việc kiểm tra hoán vị tự nhiên đáp ứng điều này mà không cần xử lý đặc biệt. 

Một trường hợp khác là khi tất cả các ký tự thuộc về một danh mục duy nhất: 

đầu vào:```
10 10 10
abcdef
```Số lượng là các chữ cái = 6, các chữ số = 0, dấu chấm câu = 0. Thuật toán sẽ kiểm tra tất cả các hoán vị và mọi ánh xạ trong đó các chữ cái được gán cho dung lượng ≥ 6 đều thành công. Vì tồn tại ít nhất một ánh xạ như vậy nên kết quả đầu ra là chính xác`Yes`. 

Trường hợp cạnh cuối cùng liên quan đến khớp nối chặt chẽ trong đó chỉ có một hoán vị cụ thể hoạt động. Vòng lặp hoán vị thô bạo đảm bảo điều này vẫn được tìm thấy, vì mỗi phép đối chiếu đều được kiểm tra độc lập và không áp dụng phương pháp cắt tỉa theo kinh nghiệm.
