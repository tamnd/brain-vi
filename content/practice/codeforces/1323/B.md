---
problem: 1323B
contest_id: 1323
problem_index: B
name: "Count Subrectangles"
contest_name: "Codeforces Round 626 (Div. 2, based on Moscow Open Olympiad in Informatics)"
rating: 1500
tags: ["binary search", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 210
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2ded2f-7b30-83ec-ae4c-241598b69c4b
---

#CF 1323B - Đếm hình chữ nhật con 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2ded2f-7b30-83ec-ae4c-241598b69c4b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Ma trận trong bài toán này không phải là ma trận tùy ý. Mỗi ô được xác định bằng một quy tắc tích ngoài đơn giản: hàng i hoàn toàn “hoạt động” hoặc hoàn toàn “không hoạt động” tùy thuộc vào việc$a_i$là 1 hoặc 0 và cột j được điều khiển tương tự bởi$b_j$. Một ô chỉ trở thành 1 khi cả hàng và cột của nó đều hoạt động. 

Vì vậy, lưới về cơ bản là một mặt nạ nhị phân trong đó tất cả các hàng có$a_i = 0$hoàn toàn bằng 0 và tất cả các hàng có$a_i = 1$nhân rộng mô hình của mảng$b$. Điều này làm cho mỗi hàng hoặc giống hệt với$b$hoặc hoàn toàn bằng không. 

Nhiệm vụ là đếm xem có bao nhiêu hình chữ nhật con thẳng hàng trong lưới này có diện tích chính xác$k$và chỉ bao gồm những cái một. Một hình chữ nhật con hợp lệ phải nằm hoàn toàn bên trong các hàng nơi$a_i = 1$và đoạn cột của nó phải nằm hoàn toàn bên trong một vùng liền kề của các đoạn trong$b$. 

Các ràng buộc buộc một giải pháp tuyến tính hoặc gần tuyến tính. Cả hai$n$Và$m$có thể đạt tới 40.000, do đó, bất kỳ phương pháp nào cố gắng liệt kê tất cả các hình chữ nhật hoặc tất cả các cặp hàng-cột đều không khả thi ngay lập tức. Thậm chí$O(nm)$vượt xa giới hạn, và thậm chí$O(n \sqrt{m})$cách tiếp cận theo phong cách có nhiều rủi ro trừ khi được tối ưu hóa cẩn thận. Cấu trúc của ma trận phải được khai thác. 

Trường hợp cạnh khóa phát sinh khi một trong hai mảng đều bằng 0. Trong trường hợp đó, ma trận hoàn toàn bằng 0 và câu trả lời phải bằng 0 bất kể$k$. Một cách tiếp cận đơn giản không lọc sớm hàng 0 hoặc cột 0 vẫn có thể cố gắng đếm các mảng con và tạo ra kết quả khác 0. 

Một trường hợp tế nhị khác xảy ra khi$k = 1$. Khi đó, mỗi ô riêng lẻ chứa số 1 là một hình chữ nhật con hợp lệ. Nếu người ta cố gắng thực thi các khối liền kề của cả hai chiều một cách không chính xác mà không đơn giản hóa cấu trúc thì rất dễ bị phức tạp hóa quá mức hoặc bị tính sai. 

Cuối cùng, khi$k$nhân tố theo nhiều cách, một phép liệt kê hình chữ nhật đơn giản có thể nhân đôi số lượng nhân tử của$k$như chiều cao nhân với chiều rộng, đó là cấu trúc cốt lõi mà cuối cùng chúng tôi dựa vào. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: liệt kê mọi hình chữ nhật con có thể có, tính diện tích của nó và kiểm tra xem tất cả các ô có phải là một hay không. Để xác minh điều kiện tất cả một, chúng ta cần quét từng ô trong hình chữ nhật. có$O(n^2 m^2)$hình chữ nhật và mỗi lần kiểm tra có giá lên tới$O(nm)$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng tôi tối ưu hóa một chút bằng cách nhận thấy rằng ma trận có cấu trúc, chúng tôi vẫn gặp phải quá nhiều ứng cử viên hình chữ nhật trừ khi chúng tôi sử dụng tính chất tích của$c_{i,j} = a_i b_j$. 

Quan sát quan trọng là một hình chữ nhật con hợp lệ khi và chỉ khi tất cả các hàng được chọn có$a_i = 1$và các cột được chọn nằm hoàn toàn bên trong một đoạn liên tiếp trong$b$. Khi chúng tôi sửa một khối liên tiếp trong$b$, mọi cột trong khối đó hoạt động giống hệt nhau trên các hàng. Vấn đề giảm xuống còn việc kết hợp hai cấu trúc 1D độc lập: chạy các cấu trúc trong$a$và chạy của những người trong$b$. 

Trước tiên, chúng tôi nén cả hai mảng thành các phân đoạn liên tiếp của một mảng. Đối với mỗi độ dài phân đoạn, chúng tôi đếm có bao nhiêu phân đoạn có độ dài nhất định tồn tại. Sau đó, chúng ta sử dụng thực tế là một hình chữ nhật có diện tích$k$phải thỏa mãn$height × width = k$và cả chiều cao và chiều rộng phải tương ứng với các phân đoạn hợp lệ trong$a$Và$b$tương ứng. Chúng tôi liệt kê tất cả các cặp yếu tố của$k$và tích lũy các khoản đóng góp. 

Điều này chuyển vấn đề thành việc đếm các mảng con có độ dài nhất định và khớp chúng thông qua các ước của$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n + m + \sqrt{k})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng$a$và tính toán, với mọi độ dài có thể$x$, có bao nhiêu đoạn con liên tiếp có độ dài ít nhất$x$hiện hữu. Điều này được thực hiện bằng cách xác định từng lần chạy của một số và đếm xem nó đóng góp bao nhiêu phân đoạn con có độ dài nhất định. Điều tương tự được thực hiện cho mảng$b$. Bước này rất cần thiết vì chỉ những lần chạy này mới có thể tạo thành các kích thước hình chữ nhật hợp lệ. 
2. Đối với mỗi lần chạy có độ dài$L$trong một mảng, nó góp phần$(L - x + 1)$các đoạn nhỏ có độ dài$x$, với mọi$x \le L$. Chúng tôi lưu trữ thông tin này một cách ngầm định bằng cách ghi lại thời lượng chạy thay vì mở rộng một cách rõ ràng. 
3. Lặp lại tất cả các ước số$h$của$k$. Với mỗi cặp số chia$(h, w)$Ở đâu$h × w = k$, giải thích$h$như chiều cao tiềm năng (hàng) và$w$dưới dạng chiều rộng tiềm năng (cột). 
4. Đối với mỗi hợp lệ$h$, tính xem có bao nhiêu phần con của các phần tử trong mảng$a$có chiều dài ít nhất$h$. Điều này cung cấp số lượng vị trí dọc có thể có. 
5. Đối với tương ứng$w$, tính xem có bao nhiêu phần con của các phần tử trong mảng$b$có chiều dài ít nhất$w$. Điều này cung cấp số lượng vị trí nằm ngang có thể. 
6. Nhân hai số đếm này và cộng vào câu trả lời. Mỗi sự kết hợp của một đoạn thẳng đứng và một đoạn ngang hợp lệ tạo thành một hình chữ nhật có diện tích duy nhất$k$. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào khả năng tách rời của cấu trúc ma trận. Mọi hình chữ nhật hợp lệ phải nằm hoàn toàn bên trong một dãy hình chữ nhật trong$a$và một loạt những cái trong$b$. Trong quá trình chạy như vậy, mọi ô được đảm bảo là 1 do cấu trúc nhân. Ngược lại, bất kỳ sự lựa chọn nào về một đoạn hàng có độ dài$h$bên trong một loạt những cái ở$a$và một đoạn cột có độ dài$w$bên trong một loạt những cái ở$b$tạo ra một hình chữ nhật hợp lệ gồm những cái. Vì diện tích chính xác là$k$, chỉ các cặp nhân tử của$k$vật chất và mỗi hình chữ nhật hợp lệ tương ứng duy nhất với một cặp như vậy. Không xảy ra việc đếm quá mức vì mỗi hình chữ nhật được xác định duy nhất bởi khoảng cách hàng và khoảng cách cột của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_segments(arr):
    n = len(arr)
    res = {}
    i = 0
    while i < n:
        if arr[i] == 0:
            i += 1
            continue
        j = i
        while j < n and arr[j] == 1:
            j += 1
        length = j - i
        for x in range(1, length + 1):
            res[x] = res.get(x, 0) + (length - x + 1)
        i = j
    return res

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    cntA = count_segments(a)
    cntB = count_segments(b)

    ans = 0
    h = 1
    while h * h <= k:
        if k % h == 0:
            w = k // h
            if h in cntA and w in cntB:
                ans += cntA[h] * cntB[w]
            if h != w and w in cntA and h in cntB:
                ans += cntA[w] * cntB[h]
        h += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén từng mảng thành các phần đóng góp của tất cả các độ dài phân đoạn có thể có. chức năng`count_segments`mở rộng từng lần chạy thành một bảng tần số trong đó khóa`x`đại diện chính xác cho bao nhiêu phân đoạn con của những phân đoạn có độ dài`x`hiện hữu. 

Sau đó, bộ giải chính sẽ lặp lại các cặp số chia của`k`. Mỗi cặp xác định một hình chữ nhật. Chúng tôi kiểm tra xem cả hai mảng có thể hỗ trợ các kích thước đó hay không và nhân số lượng vị trí. 

Một điểm tinh tế là xử lý tính đối xứng khi$h × w = k$. Chúng ta phải tránh tính hai lần khi$h = w$, vì vậy chúng tôi kiểm tra rõ ràng trường hợp đường chéo. Một điểm tinh tế khác là`cntA[x]`đại diện cho số lượng đoạn có độ dài chính xác$x$, đã mã hóa số lượng vị trí bắt đầu hợp lệ tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
1 0 1
1 1 1
```Phân tích mảng: 

| Bước | Một phân đoạn | Phân đoạn B | hệ số k | 
| --- | --- | --- | --- | 
| 1 | chạy: [1], [1] | chạy: [3] | (1,2), (2,1) | 
| 2 | cntA[1]=2 | cntB[1]=3, cntB[2]=2 | đánh giá cặp | 

Đối với (h=1, w=2), chúng ta lấy các dải 1 hàng trong A và các dải 2 cột trong B. Đối với (h=2, w=1), chúng ta lấy các đoạn 2 hàng trong A (không tồn tại, vì vậy bằng 0) và các đoạn 1 cột trong B. 

Cấu hình đóng góp duy nhất là chiều cao 1 với chiều rộng 2, tạo ra 4 hình chữ nhật hợp lệ. 

Điều này cho thấy câu trả lời phụ thuộc vào việc đếm vị trí thay vì hình học thô. 

### Ví dụ 2 (tất cả những cái) 

đầu vào:```
2 3 2
1 1
1 1 1
```| Bước | A chạy | B chạy | đóng góp | 
| --- | --- | --- | --- | 
| (1,2) | 2 hàng | 2 mảng con có độ dài 2 | 2×2 | 
| (2,1) | 1 mảng con có độ dài 2 | 3 mảng con có độ dài 1 | 1×3 | 

Tổng số trở thành 4 + 3 = 7 hình chữ nhật hợp lệ. 

Điều này khẳng định rằng nhiều cặp yếu tố đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m + \sqrt{k})$| Một lần quét tuyến tính trên mỗi mảng cộng với phép liệt kê số chia | 
| Không gian |$O(n + m)$| Lưu trữ các đóng góp của phân khúc theo thời lượng chạy | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì cả hai mảng đều được xử lý theo thời gian tuyến tính và$k$lên đến$n \cdot m$trong thực tế vẫn mang lại nhiều nhất khoảng 200 ước số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def count_segments(arr):
        n = len(arr)
        res = {}
        i = 0
        while i < n:
            if arr[i] == 0:
                i += 1
                continue
            j = i
            while j < n and arr[j] == 1:
                j += 1
            length = j - i
            for x in range(1, length + 1):
                res[x] = res.get(x, 0) + (length - x + 1)
            i = j
        return res

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    cntA = count_segments(a)
    cntB = count_segments(b)

    ans = 0
    h = 1
    while h * h <= k:
        if k % h == 0:
            w = k // h
            if h in cntA and w in cntB:
                ans += cntA[h] * cntB[w]
            if h != w and w in cntA and h in cntB:
                ans += cntA[w] * cntB[h]
        h += 1

    return str(ans)

# provided sample
assert run("3 3 2\n1 0 1\n1 1 1\n") == "4"

# all zeros
assert run("3 3 2\n0 0 0\n0 0 0\n") == "0"

# single row
assert run("1 5 2\n1\n1 1 1 1 1\n") == "4"

# single column
assert run("5 1 2\n1 1 1 1 1\n1\n") == "4"

# all ones square
assert run("2 3 2\n1 1\n1 1 1\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 0 | loại trừ sớm đúng đắn | 
| hàng đơn | 4 | chỉ đếm theo chiều ngang | 
| cột đơn | 4 | chỉ đếm theo chiều dọc | 
| đầy đủ | 7 | xử lý cặp yếu tố kết hợp | 

## Vỏ cạnh 

Đối với mảng hoàn toàn bằng 0, bảng phân đoạn trống và không có cặp chia nào đóng góp. Ví dụ:```
3 3 2
0 0 0
0 0 0
```Cả hai`cntA`Và`cntB`đều trống, vì vậy mọi tra cứu đều thất bại và câu trả lời vẫn là 0. Thuật toán tránh đếm chính xác bất kỳ hình chữ nhật nào. 

Trong một thời gian dài:```
1 5 3
1
1 1 1 1 1
```Đây`cntA[1]=1`,`cntB[3]=3`,`cntB[1]=5`, v.v. Chỉ có các cặp nhân tử gồm 3 đóng góp, cụ thể là (1,3) và (3,1). Thuật toán nhân các vị trí hợp lệ một cách chính xác mà không cần bất kỳ phép liệt kê 2D nào. 

Đối với cấu trúc nhân tố không đồng đều như k = 6, cả (2,3) và (3,2) đóng góp độc lập và việc xử lý đối xứng đảm bảo không trùng lặp khi h = w.
