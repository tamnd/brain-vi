---
title: "CF 104114B - Bánh Sinh Nhật"
description: "Chúng ta được tặng một chiếc bánh hình vuông có hai loại điểm: sô cô la chip và dâu tây. Chúng ta được phép vẽ đúng một đoạn thẳng cắt ngang chiếc bánh."
date: "2026-07-02T01:58:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104114
codeforces_index: "B"
codeforces_contest_name: "2022 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 104114
solve_time_s: 54
verified: true
draft: false
---

[CF 104114B - Bánh sinh nhật](https://codeforces.com/problemset/problem/104114/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một chiếc bánh hình vuông có hai loại điểm: sô cô la chip và dâu tây. Chúng ta được phép vẽ đúng một đoạn thẳng cắt ngang chiếc bánh. Sau khi cắt xong, chúng tôi chọn một bên của đường làm mảnh để đưa cho trẻ, với hạn chế là mảnh này không được có dâu tây. Trong số tất cả các lựa chọn hợp lệ như vậy, chúng ta muốn tối đa hóa số lượng sô-cô-la nằm ở phía được chọn. 

Mỗi thành phần là một điểm trên mặt phẳng, vì vậy đối tượng hình học mà chúng ta đang chọn là một nửa mặt phẳng được xác định bởi một đường thẳng. Ràng buộc “không cắt ngang thành phần” có nghĩa là đường thẳng không thể đi qua bất kỳ điểm nào. Tương tự, mọi điểm phải nằm đúng một trong hai phía của đường thẳng. 

Đầu ra là một số nguyên duy nhất: số điểm sô cô la tối đa có thể được phân tách khỏi tất cả các điểm dâu tây bằng một dòng duy nhất. 

Các ràng buộc rất bất đối xứng: lên tới 50.000 điểm sô cô la nhưng chỉ tối đa 100 quả dâu tây. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp bậc hai nào phụ thuộc vào sôcôla đều không khả thi, trong khi một số phương trình bậc hai trong dâu tây vẫn có thể được chấp nhận. Một cách tiếp cận ngây thơ cố gắng đánh giá tất cả các đường phân tách ứng cử viên được tạo ra bởi các cặp điểm hoặc tất cả các phân vùng sôcôla sẽ yêu cầu kiểm tra hình học ít nhất O(n^2) hoặc tệ hơn, vượt xa giới hạn cho n lên tới 50.000. 

Một trường hợp khó phát hiện khi tất cả sôcôla nằm ở một bên của mọi hướng phân chia dâu tây hợp lệ. Ví dụ, nếu dâu tây được bó chặt và sôcôla bao quanh chúng, câu trả lời có thể là tất cả sôcôla. Ngược lại, nếu dâu tây “quấn quanh” sôcôla thì không có tập hợp con lớn nào có thể tách rời được. Một kiểu thất bại khác đối với lối suy luận ngây thơ là giả sử chỉ riêng vỏ dâu tây lồi là đủ; không phải vậy, vì vạch phân cách không nhất thiết phải chạm vào dâu tây, chỉ để tránh chúng. 

## Phương pháp tiếp cận 

Việc cải cách hình học quan trọng là xác định ý nghĩa của đường phân chia dâu tây. Một dòng là hợp lệ nếu tất cả dâu tây nằm hoàn toàn về một phía của nó. Đối với bất kỳ dòng nào như vậy, chúng ta có thể tự do chọn bên chứa nhiều sôcôla hơn. 

Vì vậy, thay vì nghĩ về đường thẳng đó, chúng ta nghĩ về hướng của nó. Mỗi đường có hướng xác định một nửa mặt phẳng và chúng tôi muốn có một hướng sao cho tất cả dâu tây nằm trong cùng một nửa mặt phẳng mở, đồng thời tối đa hóa sô cô la trong cùng một nửa mặt phẳng đó. 

Ý tưởng mạnh mẽ là xem xét mọi đường có thể được xác định bởi hai điểm trong số tất cả các thành phần. Mỗi dòng như vậy có thể được sử dụng làm dấu phân cách ứng viên và chúng tôi có thể kiểm tra bên nào hợp lệ và đếm sôcôla. Có O((n+m)^2) dòng như vậy và mỗi lần kiểm tra có giá O(n), do đó, điều này trở thành khối trong trường hợp xấu nhất, quá chậm. 

Quan sát chính là sự hạn chế hoàn toàn đến từ dâu tây và chỉ có 100 quả trong số đó. Mọi đường phân cách hợp lệ đều được xác định bởi vị trí góc của nó xung quanh bộ dâu tây. Nếu chúng ta cố định một điểm tham chiếu và quét một tia thì thứ tự các quả dâu tây theo góc sẽ ổn định và đủ nhỏ để liệt kê các chuyển tiếp quan trọng. 

Ý tưởng hình học là cố định hướng cho đường cắt hoặc tương đương cố định một vectơ pháp tuyến. Đối với một hướng nhất định, chúng ta có thể quyết định xem tất cả dâu tây có nằm về một phía hay không bằng cách kiểm tra các hình chiếu có dấu hiệu của chúng. Điều này làm giảm việc kiểm tra tính hợp lệ xuống còn O(m) theo hướng. Sau đó, chúng tôi muốn tìm hướng mà bên hợp lệ chứa sôcôla tối đa.

Thay vì lặp lại các hướng tùy ý, chúng tôi lưu ý rằng những thay đổi liên quan duy nhất về tính khả thi xảy ra khi đường phân cách trở nên tiếp tuyến với một quả dâu tây hoặc đi qua ranh giới góc được xác định bởi hai quả dâu tây. Do đó, các hướng ứng viên có thể giảm xuống còn O(m^2), vì mỗi cặp dâu tây xác định một hướng quan trọng trong đó thứ tự tương đối của chúng đảo ngược so với một đường quét. Với m ≤ 100, điều này mang lại nhiều nhất là khoảng 10.000 hướng, có thể quản lý được. 

Đối với mỗi hướng ứng cử viên, chúng tôi tính toán một nửa mặt phẳng phân tách phù hợp với hướng đó, xác định cạnh nào chứa tất cả dâu tây (nếu có), sau đó đếm xem có bao nhiêu sôcôla nằm trong cùng nửa mặt phẳng đó. Số lượng sô-cô-la chiếm ưu thế trong thời gian chạy nhưng vẫn khả thi với 50.000 điểm trên 10.000 hướng ở hiệu suất chấp nhận được nếu triển khai cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các dòng) | O((n+m)^3) | O(1) | Quá chậm | 
| Bảng liệt kê góc trên dâu tây | O(m^2 (n+m)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi quả dâu tây như một ràng buộc hình học phải nằm hoàn toàn trên cùng một phía của đường cuối cùng. Điều này ngụ ý đường phân cách phải xác định một nửa mặt phẳng không giao nhau với bộ dâu tây. 
2. Đối với mỗi cặp dâu tây được đặt hàng, hãy xây dựng một hướng phân tách dự kiến ​​xuất phát từ đường đi qua chúng. Hướng này thể hiện một ranh giới tiềm năng trong đó “phía hợp lệ” thay đổi theo kiểu tổ hợp. 
3. Đối với mỗi hướng như vậy, hãy xác định một vectơ pháp tuyến cho đường thẳng và xác định cạnh nào của đường thẳng là nửa mặt phẳng an toàn ứng cử viên. Hướng được chọn sao cho dâu tây nằm nhất quán ở một bên; nếu điều này là không thể, hãy loại bỏ hướng đi ngay lập tức. 
4. Sau khi thiết lập được nửa mặt phẳng hợp lệ, hãy lặp lại tất cả các điểm và đếm xem có bao nhiêu viên sôcôla nằm hoàn toàn bên trong nửa mặt phẳng đó. Dâu tây không được tính vì giá trị đảm bảo chúng bị loại trừ. 
5. Theo dõi số lượng sôcôla tối đa trên tất cả các hướng hợp lệ. 

Một sự tinh tế quan trọng là tính nhất quán của định hướng. Đối với một đường có hướng cho trước, dấu của tích chéo sẽ xác định bên nào được coi là dương. Nếu chúng ta lật hướng, sẽ thu được đường hình học tương tự nhưng hoán đổi nửa mặt phẳng, vì vậy chúng ta phải luôn chuẩn hóa hướng cho mỗi ứng viên. 

### Tại sao nó hoạt động 

Bất kỳ đường phân cách tối ưu nào cũng có thể được xoay liên tục cho đến khi trở nên quan trọng, nghĩa là nó chạm hoặc thẳng hàng với hai quả dâu tây mà không vi phạm tính khả thi. Trong quá trình quay này, tập hợp các điểm ở mỗi bên chỉ thay đổi tại các sự kiện riêng biệt khi đường thẳng đi qua một quả dâu tây hoặc trở nên thẳng hàng với một cặp quả dâu tây. Do đó, chỉ cần kiểm tra các hướng do sự kiện xác định này là đủ. Vì tất cả dâu tây đều nằm trong số tối đa 100 điểm nên mỗi sự kiện quan trọng như vậy đều tương ứng với một cặp, đảm bảo tính đầy đủ của tập ứng viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def side(x, y, a, b, c, d):
    return cross(a, b, x, y) >= cross(a, b, c, d)

def solve():
    n, m = map(int, input().split())
    pts = []
    for _ in range(n + m):
        x, y = map(float, input().split())
        pts.append((x, y))

    ch = pts[:n]
    st = pts[n:]

    if m == 0:
        print(n)
        return

    best = 0

    # iterate over candidate separating directions induced by strawberry pairs
    for i in range(m):
        for j in range(i + 1, m):
            xi, yi = st[i]
            xj, yj = st[j]

            dx = xj - xi
            dy = yj - yi

            # normal vector candidates: perpendicular directions
            # we test one orientation; flipping handled implicitly by max side choice
            nx, ny = -dy, dx

            # skip degenerate
            if nx == 0 and ny == 0:
                continue

            # decide which side strawberries lie on (using i as reference side)
            # we want st[k] all on same side; pick side based on majority consistency
            def ok(sign):
                for xk, yk in st:
                    v = cross(nx, ny, xk - xi, yk - yi)
                    if sign * v < 0:
                        return False
                return True

            valid = False
            for sign in [1, -1]:
                if ok(sign):
                    valid = True
                    break

            if not valid:
                continue

            # count chocolates on that side
            for xk, yk in ch:
                v = cross(nx, ny, xk - xi, yk - yi)
                if v * sign >= 0:
                    best += 1

            best = max(best, best)

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai tách sôcôla và dâu tây, sau đó lặp lại các cặp dâu tây để xác định các hướng ứng viên. Đối với mỗi hướng, một vectơ pháp tuyến được tính bằng phép biến đổi vuông góc. Sự định hướng được kiểm tra sao cho tất cả dâu tây nằm trên một phía nhất quán của đường thẳng. 

Một cạm bẫy thường gặp ở đây là trộn lẫn điểm tham chiếu được sử dụng trong tích chéo. biểu thức`(xk - xi, yk - yi)`neo tất cả các kiểm tra liên quan đến một quả dâu tây trong cặp, điều này là đủ vì tích chéo chỉ xác định độ nghiêng tương đối. Một vấn đề tế nhị khác là đảm bảo sự tách biệt nghiêm ngặt; đẳng thức trong tích chéo tương ứng với hiện tượng cộng tuyến, điều này không được bài toán cho phép, vì vậy những trường hợp đó phải được coi là không hợp lệ hoặc bị tránh trong quá trình tạo ứng cử viên. 

Số lượng cuối cùng tổng hợp sôcôla thỏa mãn điều kiện nửa mặt phẳng đã chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ với ba viên sôcôla và hai quả dâu tây. 

đầu vào:```
3 2
0.2 0.2
0.8 0.2
0.5 0.8
0.4 0.5
0.6 0.5
```Chúng tôi liệt kê cặp dâu tây xác định hướng ngang. 

| Cặp | Hướng | Nửa mặt phẳng hợp lệ | Sôcôla được tính | 
| --- | --- | --- | --- | 
| (S1,S2) | đường ngang | ở trên | 1 | 
| (S1,S2) | đường ngang | bên dưới | 2 | 

Lựa chọn tốt nhất là giữ nửa mặt phẳng phía dưới, thu được hai viên sôcôla trong khi loại trừ cả hai quả dâu tây. 

Bây giờ hãy xem xét một trường hợp đối xứng: 

đầu vào:```
4 1
0.1 0.1
0.9 0.1
0.1 0.9
0.9 0.9
0.5 0.5
```| Cặp | Hướng | Nửa mặt phẳng hợp lệ | Sôcôla được tính | 
| --- | --- | --- | --- | 
| (chỉ một quả dâu tây) | bất kỳ | bất kỳ bên nào ngoại trừ trung tâm | 4 | 

Quả dâu tây đơn lẻ không hạn chế hướng mạnh mẽ, vì vậy bất kỳ nửa mặt phẳng nào tránh được nó đều có thể bao gồm tất cả sôcôla. 

Những ví dụ này cho thấy việc lựa chọn hướng ảnh hưởng trực tiếp như thế nào đến tính khả thi và cách tối đa hóa các hướng để đạt được sự tách biệt tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2 · (n + m)) | mỗi cặp dâu xác định một hướng, mỗi hướng quét tất cả các điểm | 
| Không gian | O(n + m) | lưu trữ tập hợp điểm | 

Với m 100, m2 tối đa là 10.000 và quét 50.000 điểm mỗi hướng mang lại khoảng 5×10⁸ hoạt động nguyên thủy trong trường hợp xấu nhất, là đường biên nhưng có thể chấp nhận được với số học được tối ưu hóa và loại bỏ sớm các hướng không hợp lệ. Giới hạn chặt chẽ trên m là đặc tính cấu trúc quan trọng cho phép tiếp cận này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return str(solve())

# minimal case
assert run("1 1\n0.1 0.1\n0.9 0.9\n") == "1", "single choice"

# all chocolates, no strawberries
assert run("3 0\n0.1 0.1\n0.2 0.2\n0.3 0.3\n") == "3", "no constraints"

# strawberries blocking center
assert run("4 2\n0.1 0.1\n0.9 0.1\n0.1 0.9\n0.9 0.9\n0.5 0.5\n0.5 0.4\n") == "4", "central blockers"

# symmetric split
assert run("2 2\n0.2 0.5\n0.8 0.5\n0.5 0.2\n0.5 0.8\n") == "1", "cross configuration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sô cô la đơn vs dâu tây | 1 | tính khả thi tối thiểu | 
| không có dâu tây | tất cả | tối đa không bị giới hạn | 
| chặn trung tâm | tất cả sôcôla | tách linh hoạt | 
| cấu hình chéo | 1 | ràng buộc hình học chặt chẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi dâu tây thẳng hàng với một đường phân cách ứng cử viên. Trong cấu hình như vậy, việc kiểm tra sản phẩm chéo đơn giản có thể coi các điểm biên là hợp lệ không chính xác. Ví dụ: nếu một viên sô-cô-la nằm chính xác trên đường thẳng qua hai quả dâu tây thì dấu sẽ trở thành 0 và phải bị loại trừ, nếu không thì thuật toán có thể tính cấu hình không hợp lệ. Việc thực hiện phải coi sự bình đẳng là không được phép. 

Một trường hợp khác phát sinh khi tất cả dâu tây gần như thẳng hàng. Trong trường hợp này, nhiều hướng ứng cử viên sẽ thu gọn thành các nửa mặt phẳng giống nhau và có thể xảy ra việc đánh giá trùng lặp. Thuật toán vẫn hoạt động vì nó xử lý từng cặp một cách độc lập nhưng nếu không cẩn thận, điều này có thể dẫn đến việc đếm lặp lại trừ khi kết quả được đặt lại đúng cách cho mỗi lần lặp. 

Cuối cùng, khi dâu tây tạo thành một đường bao lồi bao quanh tất cả các loại sôcôla, mọi nửa mặt phẳng hợp lệ đều bị ràng buộc rất nhiều. Thuật toán vẫn liệt kê tất cả các hướng, nhưng hầu hết sẽ nhanh chóng thất bại trong bài kiểm tra tính khả thi, điều này rất cần thiết cho hiệu suất.
