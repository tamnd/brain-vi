---
title: "CF 1546B - AquaMoon và sợi dây bị đánh cắp"
description: "Chúng ta có một tập hợp nhiều chuỗi gốc và một tập hợp thứ hai được hình thành sau một quá trình xáo trộn. Sự xáo trộn diễn ra trong hai giai đoạn. Đầu tiên, tất cả các chuỗi đều được ghép nối ngoại trừ một chuỗi đặc biệt không được ghép nối."
date: "2026-06-14T19:40:57+07:00"
tags: ["codeforces", "competitive-programming", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1546
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 732 (Div. 2)"
rating: 1200
weight: 1546
solve_time_s: 448
verified: true
draft: false
---

[CF 1546B - AquaMoon và chuỗi bị đánh cắp](https://codeforces.com/problemset/problem/1546/B) 

**Đánh giá:** 1200 
**Tags:** tương tác, toán học 
**Thời gian giải:** 7 phút 28 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp nhiều chuỗi gốc và một tập hợp thứ hai được hình thành sau một quá trình xáo trộn. Sự xáo trộn diễn ra trong hai giai đoạn. Đầu tiên, tất cả các chuỗi đều được ghép nối ngoại trừ một chuỗi đặc biệt không được ghép nối. Sau đó, đối với mỗi cặp, hai chuỗi được “trộn” một phần bằng cách hoán đổi các ký tự ở một số vị trí đã chọn và cuối cùng chuỗi không ghép đôi bị loại bỏ. Danh sách kết quả bị xáo trộn nên thứ tự không mang thông tin. 

Chúng tôi cũng được cung cấp bộ gốc đầy đủ và bộ bị hỏng cuối cùng. Nhiệm vụ là xác định chuỗi gốc nào đã biến mất hoàn toàn. 

Hạn chế cơ cấu chính là$n$là số lẻ, vì vậy chính xác một chuỗi gốc không có chuỗi đối ứng nào trong tập hợp cuối cùng. Mỗi chuỗi gốc khác tham gia vào chính xác một cặp và mỗi cặp chỉ hoán đổi các ký tự theo vị trí mà không thay đổi nhiều bộ ký tự trên cặp ở bất kỳ vị trí cố định nào. Điều đó ngụ ý một nguyên tắc bảo toàn: trên mỗi cặp, mỗi cột đóng góp tổng số ký tự giống nhau trước và sau khi hoán đổi, chỉ được phân phối lại giữa hai chuỗi. 

Giới hạn lớn: tổng cộng$\sum n \cdot m \le 10^5$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào so sánh mọi chuỗi gốc với mọi chuỗi cuối cùng, vì đó sẽ là bậc hai về số lượng chuỗi. Ngay cả việc băm toàn bộ chuỗi cho mỗi lần so sánh vẫn sẽ yêu cầu tổng hợp tuyến tính cẩn thận thay vì khớp từng cặp. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta thử kết hợp tham lam các chuỗi giống hệt nhau. Hãy xem xét ba chuỗi trong đó hai chuỗi ban đầu giống hệt nhau nhưng bị hoán đổi nhiều theo cặp. Sau khi hoán đổi, chúng có thể không còn khớp chính xác với nhau hoặc bất kỳ chuỗi gốc nào nữa, mặc dù chúng vẫn tương ứng với các phép biến đổi ghép đôi hợp lệ. Vì vậy, kết hợp bình đẳng là không đủ; chúng ta cần một bất biến cấu trúc có thể tồn tại khi hoán đổi. 

Một cạm bẫy khác là giả sử tần số của toàn bộ chuỗi hoặc số lượng ký tự là đủ. Hoán đổi theo cặp duy trì tổng số ký tự trên toàn cầu chứ không phải trên mỗi chuỗi, do đó không thể phát hiện chuỗi bị đánh cắp chỉ từ tần số chung. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử loại bỏ từng ứng cử viên chuỗi gốc và kiểm tra xem chuỗi còn lại có$n-1$các chuỗi gốc có thể được phân chia thành các cặp có thể tạo ra nhiều tập hợp cuối cùng thông qua một số thao tác hoán đổi hợp lệ. Điều này biến thành việc cố gắng khớp các cặp đã biến đổi, yêu cầu kiểm tra tất cả các cặp và tất cả cấu hình hoán đổi một cách hiệu quả. Số lượng cặp đôi là giai thừa$n$và thậm chí việc xác minh một cặp đơn lẻ cũng yêu cầu so sánh các chuỗi có độ dài$m$. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng ta không cần phải xây dựng lại các cặp hoặc hoán đổi. Thay vào đó, chúng tôi khai thác tính chẵn lẻ của số lần xuất hiện trên các cột. Mỗi lần hoán đổi trong một cặp ảnh hưởng đến chính xác hai chuỗi tại một vị trí, do đó, đối với mọi vị trí cố định và bất kỳ ký tự nào, số lần xuất hiện trên tất cả$n-1$chuỗi cuối cùng khác với chuỗi gốc ở sự đóng góp chính xác của chuỗi bị đánh cắp. Mọi thứ khác hủy bỏ theo cặp. 

Vì vậy, đối với mỗi vị trí, chúng tôi so sánh các ký tự khác nhau như thế nào giữa nhiều bộ gốc và nhiều bộ cuối cùng. Vì tất cả các chuỗi không bị đánh cắp xuất hiện trong cả hai bộ (có thể được hoán vị và thay đổi chỉ bằng cách hoán đổi bên trong các cặp), nên tổng đóng góp của chúng trên tất cả các chuỗi sẽ bị hủy bỏ theo cách có cấu trúc, để lại một dư lượng nhất quán duy nhất tương ứng với chuỗi bị thiếu. 

Điều này gợi ý việc duy trì bảng tần số cho mỗi vị trí. Đối với mỗi cột$j$, chúng tôi tính toán mức hủy giống XOR bằng cách sử dụng số lượng chữ cái. Chuỗi bị đánh cắp là chuỗi duy nhất tạo ra sự mất cân bằng toàn cầu trên tất cả các vị trí cùng một lúc. Bằng cách tích lũy sự khác biệt ở mỗi vị trí, chúng tôi khôi phục chính xác các ký tự của chuỗi bị thiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết ghép nối Brute Force | Hàm mũ | Cao | Quá chậm | 
| Hủy tần số trên mỗi vị trí |$O(nm)$|$O(26m)$hoặc$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng số lượng ký tự tần số cho từng vị trí trên tất cả các chuỗi gốc. Đối với mỗi vị trí$j$, chúng tôi lưu trữ số lần mỗi chữ cái xuất hiện trong cột$j$trong số tất cả$n$dây. Điều này đại diện cho trạng thái ban đầu. 
2. Xây dựng số lượng tần số giống nhau cho$n-1$những chuỗi cuối cùng. Điều này thể hiện trạng thái hậu xáo trộn sau tất cả các lần hoán đổi và loại bỏ. 
3. Đối với từng vị trí$j$, tính toán sự khác biệt giữa mảng tần số ban đầu và cuối cùng. Vì việc hoán đổi chỉ hoán vị các ký tự giữa các chuỗi được ghép nối ở cùng một vị trí, nên tổng số tập hợp cho mỗi vị trí trên tất cả các chuỗi không bị đánh cắp vẫn nhất quán. Sự khác biệt duy nhất đến từ chuỗi bị thiếu. 
4. Đối với từng vị trí$j$, hãy tìm ký tự có số lượng trong tập hợp ban đầu lớn hơn trong tập hợp cuối cùng. Ký tự đó phải là ký tự của chuỗi bị đánh cắp tại vị trí$j$, bởi vì đó là phần đóng góp duy nhất không được tính đến trong tập hợp cuối cùng. 
5. Nối các ký tự này trên tất cả các vị trí để xây dựng lại chuỗi bị đánh cắp. 

Bước quan trọng là cách ly từng cột. Mặc dù hoán đổi các ký tự xáo trộn theo cặp nhưng chúng không bao giờ di chuyển các ký tự qua các vị trí, do đó mỗi cột hoạt động độc lập như một hệ thống bảo tồn nhiều bộ. 

### Tại sao nó hoạt động 

Ở mọi vị trí$j$, hãy xem xét nhiều tập ký tự trên tất cả các chuỗi gốc. Sau khi ghép nối và hoán đổi, mỗi chuỗi không bị đánh cắp vẫn đóng góp chính xác một ký tự vào cột đó trong mảng cuối cùng. Hoán đổi chỉ trao đổi những đóng góp này theo cặp, duy trì sự kết hợp nhiều chuỗi của các chuỗi được ghép nối. Do đó, khi so sánh các cột gốc và cột cuối cùng, tất cả các đóng góp được ghép nối đều bị hủy hoàn toàn, để lại chính xác một ký tự không khớp trên mỗi cột, ký tự này phải thuộc về chuỗi bị đánh cắp. Vì chuỗi bị đánh cắp nhất quán trên tất cả các cột nên các ký tự không khớp này sẽ tập hợp thành một chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        orig = [input().strip() for _ in range(n)]
        final = [input().strip() for _ in range(n - 1)]

        cnt_o = [[0] * 26 for _ in range(m)]
        cnt_f = [[0] * 26 for _ in range(m)]

        for s in orig:
            for j, ch in enumerate(s):
                cnt_o[j][ord(ch) - 97] += 1

        for s in final:
            for j, ch in enumerate(s):
                cnt_f[j][ord(ch) - 97] += 1

        res = []
        for j in range(m):
            for c in range(26):
                if cnt_o[j][c] > cnt_f[j][c]:
                    res.append(chr(c + 97))
                    break

        print("".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp được thực hiện bằng cách xây dựng hai bảng tần số theo cột. Các vòng lặp lồng nhau trên các chuỗi và ký tự trực tiếp thực hiện việc tổng hợp theo từng cột. Logic trừ là ẩn: chúng tôi xác định vị trí ký tự có số đếm giảm chính xác một giữa trạng thái ban đầu và cuối cùng. 

Một lỗi triển khai phổ biến là quên rằng mỗi cột phải được xử lý độc lập. Việc xử lý chuỗi dưới dạng toàn bộ đơn vị sẽ làm mất cấu trúc hủy bỏ được đưa ra bởi các giao dịch hoán đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
aaaaa
bbbbb
ccccc
aaaaa
bbbbb
```Chúng tôi tính toán tần số cột. 

| Cột | Số gốc | Đếm cuối cùng | Sự khác biệt | Ký tự được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | một:3 | một:2 | một:1 | một | 
| 2 | b:3 | b:2 | b:1 | b | 
| 3 | c:3 | c:2 | c:1 | c | 
| 4 | c:3 | c:2 | c:1 | c | 
| 5 | c:3 | c:2 | c:1 | c | 

Điều này tái tạo lại`"ccccc"`. 

Dấu vết này cho thấy ngay cả khi xảy ra hoán đổi, sự mất cân bằng trên mỗi cột vẫn tách biệt phần đóng góp còn thiếu. 

### Ví dụ 2 

đầu vào:```
5 6
abcdef
uuuuuu
kekeke
ekekek
xyzklm
xbcklf
eueueu
ayzdem
ukukuk
```Chúng tôi lại so sánh tần số theo cột và trích xuất những đóng góp còn thiếu. 

| Cột | Nhân vật được chọn | 
| --- | --- | 
| 1 | k | 
| 2 | e | 
| 3 | k | 
| 4 | e | 
| 5 | k | 
| 6 | e | 

Kết quả là`"kekeke"`. 

Điều này chứng tỏ rằng ngay cả những hoán đổi được xen kẽ nhiều trên nhiều cặp cũng không ảnh hưởng đến việc phát hiện sự mất cân bằng trên mỗi cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| mỗi ký tự được xử lý một lần cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(26m)$| mảng tần số trên mỗi cột | 

Tổng giới hạn kích thước đầu vào$\sum n \cdot m \le 10^5$đảm bảo thuật toán chạy thoải mái trong giới hạn vì mọi thao tác đều có thời gian không đổi cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out_lines = []
    for _ in range(t):
        n, m = map(int, input().split())
        orig = [input().strip() for _ in range(n)]
        final = [input().strip() for _ in range(n - 1)]

        cnt_o = [[0]*26 for _ in range(m)]
        cnt_f = [[0]*26 for _ in range(m)]

        for s in orig:
            for j, ch in enumerate(s):
                cnt_o[j][ord(ch)-97] += 1
        for s in final:
            for j, ch in enumerate(s):
                cnt_f[j][ord(ch)-97] += 1

        res = []
        for j in range(m):
            for c in range(26):
                if cnt_o[j][c] > cnt_f[j][c]:
                    res.append(chr(c+97))
                    break

        out_lines.append("".join(res))

    return "\n".join(out_lines)

# provided sample
assert run("""3
3 5
aaaaa
bbbbb
ccccc
aaaaa
bbbbb
3 4
aaaa
bbbb
cccc
aabb
bbaa
5 6
abcdef
uuuuuu
kekeke
ekekek
xyzklm
xbcklf
eueueu
ayzdem
ukukuk
""") == """ccccc
cccc
kekeke"""

# minimum size
assert run("""1
1 1
a
""") == """a"""

# all equal strings
assert run("""1
3 3
aaa
aaa
bbb
aaa
aaa
""") == """bbb"""

# single differing column
assert run("""1
3 3
abc
def
ghi
abc
def
""") == """ghi"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | một | ranh giới tối thiểu | 
| chuỗi lặp lại | bbb | tính chính xác của việc khử tần số | 
| lưới bảng chữ cái có cấu trúc | ghi | độc lập cho mỗi cột | 

## Vỏ cạnh 

Khi tất cả các chuỗi đều giống hệt nhau ngoại trừ chuỗi bị đánh cắp, sự khác biệt về tần số sẽ tập trung hoàn toàn vào mẫu lặp lại đó. Thuật toán vẫn tách biệt từng cột một cách độc lập, do đó chuỗi bị đánh cắp được xây dựng lại một cách chính xác ngay cả khi không tồn tại sự phân biệt theo cặp. 

Khi hoán đổi theo cặp là tối đa, nghĩa là mọi vị trí trong mỗi cặp đều bị hoán đổi, các chuỗi riêng lẻ sẽ bị biến dạng nặng nề. Tuy nhiên, nhiều tập hợp theo cột vẫn bất biến giữa các đóng góp được ghép nối, do đó, logic mất cân bằng vẫn cô lập chuỗi bị thiếu mà không dựa vào bất kỳ sự tương đồng về cấu trúc nào giữa chuỗi gốc và chuỗi cuối cùng. 

Khi$n = 1$, mảng cuối cùng trống. Mỗi chênh lệch tần số của cột bằng chuỗi gốc duy nhất, do đó thuật toán trả về trực tiếp chuỗi đó mà không có sự mơ hồ.
