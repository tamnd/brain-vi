---
title: "CF 1367E - Dây chuyền lắp ráp"
description: "Chúng ta được cấp một tập hợp nhiều hạt màu, trong đó mỗi hạt chỉ là một chữ cái viết thường. Từ những hạt này, chúng tôi muốn chọn một số tập hợp con và sắp xếp nó thành một vòng tròn, tạo thành một chiếc vòng cổ. Sự sắp xếp là hình tròn nên việc quay rất quan trọng nhưng không có điểm bắt đầu cố định."
date: "2026-06-18T18:20:43+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "graphs", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1367
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 650 (Div. 3)"
rating: 1900
weight: 1367
solve_time_s: 68
verified: true
draft: false
---

[CF 1367E - Lắp ráp vòng cổ](https://codeforces.com/problemset/problem/1367/E) 

**Xếp hạng:** 1900 
**Tags:** vũ lực, dfs và tương tự, dp, đồ thị, tham lam, lý thuyết số 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều hạt màu, trong đó mỗi hạt chỉ là một chữ cái viết thường. Từ những hạt này, chúng tôi muốn chọn một số tập hợp con và sắp xếp nó thành một vòng tròn, tạo thành một chiếc vòng cổ. Sự sắp xếp là hình tròn nên việc quay rất quan trọng nhưng không có điểm bắt đầu cố định. 

Một chiếc vòng cổ được gọi là k-đẹp nếu dịch chuyển từng hạt về phía trước k vị trí dọc theo vòng tròn mà chiếc vòng cổ không thay đổi. Nói cách khác, nếu bạn xoay vòng tròn k bước, chuỗi màu sẽ giống hệt với những gì bạn đã bắt đầu. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi muốn số lượng hạt tối đa có thể có trong cách sắp xếp hình tròn k đẹp như vậy, tuân theo ràng buộc là chúng tôi chỉ có thể sử dụng các hạt có sẵn trong nhiều tập hợp nhất định. 

Đầu vào chính là một chuỗi mô tả các hạt có sẵn và tham số k xác định yêu cầu đối xứng quay. Đầu ra là một số nguyên duy nhất, độ dài lớn nhất của cách sắp xếp hình tròn thỏa mãn ràng buộc tính đối xứng và tính khả thi này. 

Các ràng buộc đủ nhỏ để tổng số hạt trong tất cả các trường hợp thử nghiệm nhiều nhất là 2000. Điều này ngay lập tức loại trừ bất kỳ điều gì tệ hơn khoảng O(n^2) cho mỗi trường hợp thử nghiệm và khiến việc thử tất cả độ dài ứng cử viên và xác minh từng hạt bằng một kiểm tra nhanh là thực tế. 

Một cách giải thích ngây thơ thường thất bại là cho rằng chúng ta chỉ đơn giản là khớp các tần số hoặc xây dựng một chuỗi tuần hoàn một cách tham lam. Ví dụ, nếu k lớn, người ta có thể cho rằng tính đối xứng không phù hợp một cách sai lầm. Hãy xem xét trường hợp n = 5, k = 4 và tất cả các hạt đều khác biệt. Một cách tiếp cận tham lam ngây thơ có thể cố gắng tạo thành độ dài 5, nhưng trên thực tế, ràng buộc đối xứng buộc một cấu trúc mạnh chỉ có thể cho phép các chu kỳ hợp lệ nhỏ hơn tùy thuộc vào phân bố tần số. 

Một trường hợp thất bại tinh vi khác là bỏ qua rằng ràng buộc vòng tròn tạo ra các chu kỳ vị trí. Ví dụ: nếu k = 2 và độ dài là 6, các vị trí được chia thành các chu kỳ như (0,2,4) và (1,3,5). Giải pháp xử lý các vị trí một cách độc lập sẽ vượt quá tính khả dụng vì mỗi chu kỳ yêu cầu các ký tự giống hệt nhau. 

## Phương pháp tiếp cận 

Chiến lược brute-force là thử mọi độ dài L có thể từ 1 đến n và kiểm tra xem liệu chúng ta có thể tạo ra một chiếc vòng cổ k-đẹp có độ dài đó hay không. Đối với mỗi L cố định, chúng tôi mô phỏng cấu trúc được áp đặt bởi quy tắc xoay. 

Thực tế cấu trúc quan trọng là việc quay theo k có nghĩa là các chỉ số được kết nối theo chu kỳ. Nếu chúng ta gắn nhãn các vị trí từ 0 đến L−1 thì vị trí i phải khớp với vị trí (i + k) mod L. Việc lặp lại ràng buộc này sẽ phân chia các chỉ số thành các chu trình rời rạc. Trong mỗi chu kỳ, tất cả các vị trí phải chứa cùng một chữ cái. 

Số chu kỳ được xác định bởi ước chung lớn nhất của L và k. Nếu chúng ta xác định g = gcd(L, k), thì có g chu trình, mỗi chu trình có kích thước m = L / g. Mỗi chu kỳ hoạt động giống như một “khe” duy nhất phải được điền bằng một chữ cái, nhưng việc lấp đầy một khe sẽ tiêu tốn m bản sao của chữ cái đó. 

Vì vậy, đối với một L cố định, vấn đề trở thành: chúng ta có các khe g, mỗi khe yêu cầu m hạt giống nhau và chúng ta có tần số chữ cái. Mỗi chữ cái c có thể đóng góp nhiều nhất vào các vị trí sàn(freq[c] / m). Chúng ta chỉ cần kiểm tra xem tổng số vị trí có thể sử dụng trên tất cả các chữ cái có ít nhất là g hay không. 

Brute-force hoạt động vì việc kiểm tra tính khả thi của một L chỉ là 26 thao tác. Điểm lỗi là vòng lặp bên ngoài trên L, chạy tới 2000, cung cấp khoảng 52.000 lượt kiểm tra cho mỗi trường hợp thử nghiệm, nằm trong giới hạn cho tổng kích thước đầu vào. 

Không cần lập trình động hoặc tìm kiếm đồ thị ngoài quan sát này, vì phân rã chu trình tuyến tính hóa hoàn toàn cấu trúc.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên L với kiểm tra tính khả thi | O(n log n) cho mỗi trường hợp thử nghiệm (bao gồm gcd) | O(1) thêm | Đã chấp nhận | 
| Tính toán trước / DP | Không cần thiết | O(1) | Quá mức cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại tất cả các độ dài vòng cổ có thể có của ứng cử viên và kiểm tra xem mỗi chiều dài có thể được xây dựng theo ràng buộc đối xứng hay không. 

1. Với độ dài ứng cử viên L, hãy tính g = gcd(L, k). Giá trị này xác định cách các vị trí được chia thành các chu kỳ độc lập khi xoay theo k. Nếu hai vị trí trong cùng một chu kỳ thì chúng phải có cùng màu. 
2. Tính m = L/g, kích thước của từng chu kỳ. Mỗi màu được chọn gán cho một chu kỳ phải cung cấp m hạt giống hệt nhau. 
3. Đếm tần số của tất cả 26 chữ cái trong chuỗi đầu vào. Điều này được tái sử dụng cho tất cả L. 
4. Với mỗi chữ cái c, hãy tính xem nó có thể hỗ trợ bao nhiêu chu kỳ đầy đủ, đó là freq[c] // m. Đây là số chu kỳ chúng ta có thể gán màu này. 
5. Tính tổng những đóng góp này trên tất cả các chữ cái. Nếu tổng số ít nhất là g thì có thể gán màu cho tất cả các chu kỳ, nghĩa là tồn tại một vòng cổ hợp lệ có độ dài L. 
6. Theo dõi mức L tối đa phù hợp với tính khả thi. 

Lý do đằng sau việc kiểm tra này là mỗi chu kỳ sẽ độc lập khi chúng ta sửa màu của nó và mỗi chu kỳ tiêu thụ chính xác m hạt giống hệt nhau. Vì vậy, tính khả thi giảm xuống còn việc đóng gói g “nhu cầu chu kỳ” giống hệt nhau vào ngân sách tần số sẵn có. 

### Tại sao nó hoạt động 

Ràng buộc xoay tạo ra sự bằng nhau dọc theo các quỹ đạo của phép biến đổi i → i + k mod L. Các quỹ đạo này chính xác là các chu trình gcd(L, k). Bất kỳ vòng cổ hợp lệ nào cũng phải không đổi trên mỗi quỹ đạo. Ngược lại, bất kỳ việc gán màu nào cho các chu kỳ đều tự động thỏa mãn ràng buộc xoay vì việc dịch chuyển theo k chỉ hoán vị các vị trí trong mỗi chu kỳ. Kiểm tra tính khả thi khớp nguồn cung (số lượng chữ cái) với nhu cầu (chu kỳ có kích thước m), do đó, nó mô tả chính xác độ dài có thể xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        freq = [0] * 26
        for ch in s:
            freq[ord(ch) - 97] += 1

        ans = 1

        for L in range(1, n + 1):
            from math import gcd
            g = gcd(L, k)
            m = L // g

            total = 0
            for f in freq:
                total += f // m

            if total >= g:
                ans = L

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp điều kiện khả thi được đưa ra trước đó. Mảng tần số được tính toán một lần cho mỗi trường hợp thử nghiệm. Đối với mỗi độ dài ứng cử viên L, chúng tôi tính toán lại cấu trúc chu trình gcd và rút ra. Vòng lặp bên trong gồm 26 chữ cái đảm bảo hoạt động liên tục trên mỗi L. 

Một điểm tinh tế là chúng tôi chưa bao giờ xây dựng chiếc vòng cổ một cách rõ ràng. Tất cả lý do được thực hiện dựa trên số chu kỳ, giúp tránh được bất kỳ vụ nổ tổ hợp nào. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản: n = 6, k = 3, s = "abcbac". 

Chúng tôi đánh giá độ dài có thể, tập trung vào L = 6. 

Với L = 6, g = gcd(6, 3) = 3, vậy có 3 chu trình, mỗi chu trình có kích thước m = 2. Mỗi chu trình cần hai hạt giống hệt nhau. Việc đếm tần số cho phép chúng tôi kiểm tra xem mỗi chữ cái có thể cung cấp bao nhiêu cặp. 

| L | g = gcd(L,k) | m = L/g | chu kỳ có thể sử dụng cho mỗi chữ cái | tổng số chu kỳ | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 3 | 2 | a:1, b:1, c:1 | 3 | vâng | 

Vì chúng tôi có thể hỗ trợ 3 chu kỳ nên độ dài 6 là có thể đạt được. 

Bây giờ hãy xem xét trường hợp n = 3, k = 6, s = "aaa". 

Với L = 3, g = gcd(3, 6) = 3, do đó m = 1. Mỗi chu kỳ là một vị trí duy nhất, do đó không có ràng buộc nào ngoài tính khả dụng. Mỗi chữ cái đóng góp freq[c] // 1 chu kỳ, tạo ra tổng cộng 3 chu kỳ, khớp với g. 

| L | g | m | tổng số chu kỳ | khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 1 | 3 | vâng | 

Điều này cho thấy khi k lớn hơn L thì cấu trúc thoái hóa thành các vị trí độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 26 · log n) cho mỗi trường hợp thử nghiệm | Chúng tôi thử từng L cho đến n, tính gcd theo thời gian ghi nhật ký và tính tổng trên 26 chữ cái | 
| Không gian | O(1) phụ trợ | Chỉ sử dụng mảng tần số có kích thước cố định | 

Tổng số n trong các trường hợp thử nghiệm nhiều nhất là 2000, do đó việc tính toán trong trường hợp xấu nhất nằm trong giới hạn. Ngay cả vòng lặp 2000 × 2000 đầy đủ cũng nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import math
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            s = input().strip()

            freq = [0] * 26
            for ch in s:
                freq[ord(ch) - 97] += 1

            ans = 1

            for L in range(1, n + 1):
                g = math.gcd(L, k)
                m = L // g

                total = 0
                for f in freq:
                    total += f // m

                if total >= g:
                    ans = L

            print(ans)

    solve()
    sys.stdout.seek(0)
    return sys.stdout.read().strip()

# provided samples
assert run("""6
6 3
abcbac
3 6
aaa
7 1000
abczgyo
5 4
ababa
20 10
aaebdbabdbbddaadaadc
20 5
ecbedececacbcbccbdec
""") == """6
3
5
4
15
10"""

# custom cases
assert run("""1
1 100
a
""") == "1", "single bead always works"

assert run("""1
2 1
ab
""") == "1", "rotation by 1 forces equality in length 2"

assert run("""1
4 2
aabb
""") == "4", "two cycles of size 2"

assert run("""1
6 2
aaabbb
""") == "6", "balanced distribution across cycles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hạt đơn | 1 | cấu trúc hợp lệ tối thiểu | 
| ab với k=1 | 1 | ràng buộc đối xứng mạnh mẽ | 
| aabb với k=2 | 4 | đóng gói đúng chu kỳ | 
| aaabbb với k=2 | 6 | sử dụng đầy đủ qua các chu kỳ | 

## Vỏ cạnh 

Trường hợp có độ dài tối thiểu như một hạt đơn chứng tỏ rằng thuật toán luôn chấp nhận L = 1 vì g = 1 và m = 1, do đó mọi mảng tần số đều thỏa mãn điều kiện một cách tầm thường. 

Trường hợp k = 1 buộc mọi vị trí trong một chu kỳ có độ dài L chỉ thu gọn thành một chu kỳ duy nhất khi L = 1. Đối với L lớn hơn, m = L và chỉ một màu có thể lấp đầy toàn bộ cấu trúc, điều này được kiểm tra tính khả thi nắm bắt chính xác vì sàn(freq[c] / L) trở thành 0 trừ khi tất cả các hạt đều giống hệt nhau. 

Trường hợp k lớn hơn L hoạt động giống như phép quay đồng nhất với g = L, biến mỗi vị trí thành một chu kỳ riêng. Thuật toán giảm một cách chính xác để kiểm tra xem tổng số hạt có sẵn có bao gồm các vị trí L hay không, có khớp với điều kiện tổng tần số được ngầm thực thi thông qua m = 1 hay không.
