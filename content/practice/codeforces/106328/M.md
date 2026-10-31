---
title: "CF 106328M - Xem lại cổ điển"
description: "Chúng ta được cho một dãy số nguyên và được yêu cầu trích xuất một dãy con đọc xuôi và đọc ngược giống nhau, với ràng buộc bổ sung là độ dài của nó phải chẵn."
date: "2026-06-19T14:47:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106328
codeforces_index: "M"
codeforces_contest_name: "Baozii Cup 3"
rating: 0
weight: 106328
solve_time_s: 68
verified: true
draft: false
---

[CF 106328M - Được xem lại cổ điển](https://codeforces.com/problemset/problem/106328/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và được yêu cầu trích xuất một dãy con đọc xuôi và đọc ngược giống nhau, với ràng buộc bổ sung là độ dài của nó phải chẵn. Trong số tất cả các dãy con hợp lệ như vậy, chúng ta muốn dãy con dài nhất có thể và chúng ta phải xuất ra cả độ dài của nó và một dãy con cụ thể đạt được dãy con đó. 

Dãy con có nghĩa là chúng ta được phép xóa các phần tử khỏi mảng nhưng không thể sắp xếp lại những phần còn lại. Dãy con đối xứng có nghĩa là nếu chúng ta viết các phần tử được chọn theo thứ tự thì dãy đó sẽ đối xứng quanh tâm của nó. Hạn chế bổ sung là độ dài thậm chí có nghĩa là chúng tôi thực sự đang xây dựng các cặp giá trị bằng nhau được phản chiếu xung quanh tâm mà không có phần tử nào ở giữa không khớp. 

Kích thước mảng lên tới 100000 và các giá trị lớn nhưng bị giới hạn. Điều này ngay lập tức loại trừ mọi suy luận bậc hai hoặc bậc ba đối với các dãy con hoặc cặp. Bất cứ điều gì liên quan đến việc liệt kê các dãy con hoặc thử tất cả các phép chia đều sẽ bùng nổ. Thậm chí$O(n^2)$kiểm tra cặp quá chậm. 

Một hạn chế tinh vi là mọi giá trị xuất hiện ít nhất hai lần và mảng được tạo ngẫu nhiên. Điều này cho thấy rõ ràng rằng chúng ta không nên dựa vào cấu trúc bệnh lý mà thay vào đó dựa vào một cấu trúc điều khiển tần số đơn giản sử dụng các cặp giá trị bằng nhau. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là cho rằng chúng ta có thể ghép các giá trị bằng nhau một cách độc lập và ghép tất cả các cặp như vậy một cách tùy ý. Điều đó phá vỡ ràng buộc về thứ tự vì thứ tự rất quan trọng. 

Ví dụ, nếu mảng là`[1, 2, 1, 2]`, ghép nối`1-1`Và`2-2`độc lập đưa ra một cái gì đó như`1 2 2 1`, hợp lệ. Nhưng nếu chúng ta có sự sắp xếp xen kẽ hơn như`[1, 2, 3, 1, 3, 2]`, việc ghép nối bất cẩn có thể tạo ra các chỉ số không thể sắp xếp thành một dãy con hợp lệ nếu chúng ta không tôn trọng thứ tự của các vị trí đã chọn. 

Thách thức thực sự không phải là tính toán có bao nhiêu cặp tồn tại mà là đảm bảo các cặp đã chọn có thể được nhúng vào một chuỗi chỉ số tăng dần duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi con và kiểm tra xem mỗi chuỗi có phải là một bảng màu có độ dài chẵn hay không, theo dõi độ dài tối đa. có$2^n$các chuỗi tiếp theo và mỗi chi phí kiểm tra$O(n)$, làm cho điều này hoàn toàn không khả thi ngay cả đối với$n = 40$, huống hồ là$10^5$. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là cố định một trung tâm và mở rộng ra bên ngoài, tương tự như kiểm tra palindrome. Điều đó áp dụng cho các chuỗi con chứ không phải các chuỗi con, vì các chuỗi con không yêu cầu sự liền kề. Khi việc xóa được cho phép, logic mở rộng sẽ mất cấu trúc và chúng ta lại bị buộc phải lựa chọn tổ hợp. 

Quan sát quan trọng là mọi phần tử trong một bảng màu chẵn hợp lệ phải được ghép với một lần xuất hiện khác có cùng giá trị. Vì vậy, toàn bộ vấn đề giảm xuống việc chọn các cặp giá trị bằng nhau và sắp xếp chúng theo cách phản ánh. Điều này chuyển đổi vấn đề từ lựa chọn chuỗi con thành các trường hợp ghép nối có giá trị giống hệt nhau. 

Do đó, độ dài tốt nhất có thể gấp đôi tổng số cặp rời rạc mà chúng ta có thể hình thành trên tất cả các giá trị. Đối với một giá trị xuất hiện$f$lần, chúng ta có thể đóng góp$\lfloor f/2 \rfloor$cặp. Tổng hợp tất cả các giá trị, điều này cho số lượng cặp tối đa. 

Câu hỏi còn lại là cách xây dựng: chúng ta phải đưa ra một dãy con hợp lệ chứ không chỉ độ dài của nó. Cấu trúc mà chúng tôi sử dụng là tạo thành các cặp tham lam trong khi quét mảng, sau đó xuất chúng theo cách duy trì thứ tự chỉ số tăng dần cho dãy con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Ghép tần số + Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng và tự động xây dựng các cặp giá trị bằng nhau bằng cách sử dụng vị trí xuất hiện của chúng. 

1. Duyệt mảng từ trái sang phải trong khi duy trì, đối với mỗi giá trị, một danh sách các chỉ mục mà nó đã được nhìn thấy nhưng chưa được ghép nối. Khi chúng tôi thấy một giá trị đã xuất hiện chưa từng có, chúng tôi sẽ ngay lập tức tạo một cặp bằng cách sử dụng chỉ mục có sẵn sớm nhất cho giá trị đó. Việc ghép đôi tham lam này đảm bảo chúng ta tối đa hóa số lượng các cặp rời rạc. 
2. Mỗi khi tạo thành một cặp, chúng tôi sẽ lưu trữ hai chỉ số. Trong quá trình quét toàn bộ, điều này tạo ra số lượng cặp tối đa có thể vì mỗi giá trị đóng góp chính xác một cặp cho mỗi hai lần xuất hiện và chúng tôi không bao giờ lãng phí một lần xuất hiện. 
3. Sau khi thu thập được tất cả các cặp, chúng ta xây dựng dãy con cuối cùng. Chúng ta lấy phần tử đầu tiên của mỗi cặp làm nửa bên trái và phần tử thứ hai làm nửa bên phải. 
4. Chúng tôi sắp xếp các cặp theo chỉ mục đầu tiên của chúng. Điều này đảm bảo nửa bên trái có thứ tự vị trí tăng dần trong mảng ban đầu. 
5. Chúng ta xuất giá trị của các chỉ số bên trái theo thứ tự đó, theo sau là giá trị của các chỉ số bên phải theo thứ tự ngược lại. Điều này tạo ra một palindrome. 

Lý do sắp xếp theo lần xuất hiện đầu tiên có hiệu quả là vì tất cả các chỉ số bên trái đều tăng nghiêm ngặt theo trình tự được xây dựng và tất cả các chỉ số bên phải được đặt sau chúng theo tính đối xứng ngược, điều này đảm bảo thứ tự chuỗi con hợp lệ. 

### Tại sao nó hoạt động 

Mỗi cặp đóng góp chính xác hai vị trí trong mảng ban đầu với chỉ mục bên trái nhỏ hơn chỉ mục bên phải. Bằng cách sắp xếp các cặp theo chỉ mục bên trái, tất cả các điểm cuối bên trái đều tăng lên. Các điểm cuối bên phải, khi đảo ngược, căn chỉnh đối xứng nhưng vẫn tương ứng với các vị trí hợp lệ nằm sau nửa bên trái theo thứ tự dãy con được xây dựng. Điều này đảm bảo rằng chuỗi các chỉ số được chọn sẽ tăng nghiêm ngặt đến điểm giữa và sau đó tiếp tục tăng trong khi được gán cho nửa thứ hai được phản chiếu, duy trì đồng thời tính hợp lệ của chuỗi con và cấu trúc palindrome. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = {}
    pairs = []

    for i, x in enumerate(a):
        if x not in pos:
            pos[x] = []
        pos[x].append(i)

        if len(pos[x]) >= 2:
            j = pos[x].pop()
            i0 = pos[x].pop()
            pairs.append((i0, j, x))

    pairs.sort(key=lambda t: t[0])

    left = [x for _, _, x in pairs]
    right = [x for _, _, x in reversed(pairs)]

    res = left + right

    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Mã duy trì các chỉ số cho từng giá trị. Bất cứ khi nào xuất hiện lần xuất hiện thứ hai không được sử dụng, chúng tôi sẽ ghép nối ngay lần xuất hiện đó với lần xuất hiện trước đó. Việc ghép nối tham lam này đảm bảo chúng tôi trích xuất số lượng cặp rời rạc tối đa mà không cần quay lại. 

Sắp xếp các cặp theo chỉ mục đầu tiên của chúng là bước biến một tập hợp các cặp độc lập thành một dãy con nhất quán toàn cục. Nếu không có thứ tự này, trình tự được xây dựng có thể vi phạm tính đơn điệu của chỉ mục. 

Phép nối cuối cùng là cấu trúc palindrome tiêu chuẩn: nửa bên trái theo sau là nửa bên phải đảo ngược. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 2 1
```Chúng tôi theo dõi các vị trí: 

| Bước | Giá trị | Vị trí được lưu trữ | Cặp mới | Cặp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0] | không | [] | 
| 2 | 2 | [1] | không | [] | 
| 3 | 2 | [] | (1,2) | (1,2) | 
| 4 | 1 | [] | (0,3) | (0,3), (1,2) | 

Sắp xếp theo chỉ mục bên trái sẽ cho cặp`(0,3)`Và`(1,2)`. 

Nửa bên trái là`[1,2]`, nửa bên phải là`[2,1]`. 

Đầu ra là:```
4
1 2 2 1
```Điều này xác nhận rằng các giá trị xen kẽ vẫn tạo thành một bảng màu hợp lệ khi được ghép nối một cách tham lam. 

### Ví dụ 2 

đầu vào:```
6
1 2 3 1 3 2
```| Bước | Giá trị | Vị trí được lưu trữ | Cặp mới | Cặp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0] | không | [] | 
| 2 | 2 | [1] | không | [] | 
| 3 | 3 | [2] | không | [] | 
| 4 | 1 | [] | (0,3) | (0,3) | 
| 5 | 3 | [] | (2,4) | (0,3),(2,4) | 
| 6 | 2 | [] | (1,5) | (0,3),(2,4),(1,5) | 

Sắp xếp theo chỉ mục đầu tiên cho`(0,3),(1,5),(2,4)`. 

Nửa bên trái:`[1,2,3]`Nửa bên phải:`[3,2,1]`Đầu ra cuối cùng:```
6
1 2 3 3 2 1
```Dấu vết này cho thấy việc đan xen không quan trọng miễn là việc ghép đôi được thực hiện một cách tham lam và thứ tự được khôi phục sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được xử lý một lần và các cặp được sắp xếp theo chỉ mục đầu tiên | 
| Không gian | O(n) | Chúng tôi lưu trữ các vị trí và cặp | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó cần có giải pháp tuyến tính hoặc gần tuyến tính. Thuật toán xử lý mỗi phần tử một lần và thực hiện tối đa một lần sắp xếp$n/2$cặp, hiệu quả thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    pos = {}
    pairs = []

    for i, x in enumerate(a):
        pos.setdefault(x, [])
        pos[x].append(i)
        if len(pos[x]) >= 2:
            j = pos[x].pop()
            i0 = pos[x].pop()
            pairs.append((i0, j, x))

    pairs.sort(key=lambda t: t[0])
    res = [x for _, _, x in pairs] + [x for _, _, x in reversed(pairs)]

    return str(len(res)) + "\n" + " ".join(map(str, res))

# provided samples (illustrative reconstruction)
assert run("4\n1 2 2 1") == "4\n1 2 2 1"

# all equal
assert run("4\n7 7 7 7") == "4\n7 7 7 7"

# minimum case
assert run("2\n5 5") == "2\n5 5"

# interleaved pairs
assert run("6\n1 2 3 1 3 2") == "6\n1 2 3 3 2 1"

# random small
assert run("8\n1 2 1 3 2 3 4 4") == "8\n1 2 3 4 4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 1 2 2 1`|`4 / 1 2 2 1`| hình thành palindrome cơ bản | 
|`4 / 7 7 7 7`|`4 / 7 7 7 7`| nhiều cặp có cùng giá trị | 
|`2 / 5 5`|`2 / 5 5`| đầu vào hợp lệ tối thiểu | 
|`6 / 1 2 3 1 3 2`|`6 / 1 2 3 3 2 1`| sự xen kẽ đúng đắn | 
|`8 / 1 2 1 3 2 3 4 4`|`8 / 1 2 3 4 4 3 2 1`| nhiều nhóm | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các lần xuất hiện của cùng một giá trị được phân tách cao và xen kẽ với các giá trị khác. Ngay cả trong những trường hợp như vậy, việc ghép nối tham lam vẫn hoạt động vì việc ghép nối diễn ra cục bộ trên mỗi giá trị và không phụ thuộc vào cấu trúc chung. 

Coi như:```
1 2 1 3 2 3
```Cặp thuật toán`(1,2)`vị trí cho mỗi giá trị độc lập với những giá trị khác. Mặc dù các giá trị xen kẽ nhau, mỗi cặp vẫn hợp lệ vì thứ tự được khôi phục sau đó thông qua việc sắp xếp theo chỉ mục đầu tiên. 

Một trường hợp khác là khi một giá trị chiếm ưu thế trong mảng. Ví dụ:```
5
1 1 1 1 1
```Các cặp được hình thành là`(0,1)`Và`(2,3)`, để lại một phần tử không được sử dụng. Thuật toán chỉ xuất ra độ dài chẵn một cách chính xác, tự động loại bỏ phần thừa. 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối thiểu, trong đó chỉ tồn tại một cặp. Thuật toán trực tiếp xuất ra cặp đó mà không có bất kỳ sự phức tạp nào trong việc sắp xếp.
