---
title: "CF 104820K - \u0412\u044b\u0431\u043e\u0440 \u043d\u0435 \u0432\u0435\u043b\u0438\u043a"
description: "Chúng ta được cung cấp một lưới có các hàng và cột có kích thước không đồng đều. Mỗi hàng có chiều cao dương do mảng A cung cấp và mỗi cột có chiều rộng dương do mảng B cung cấp."
date: "2026-06-28T12:58:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "K"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 103
verified: true
draft: false
---

[CF 104820K - \u0412\u044b\u0431\u043e\u0440 \u043d\u0435 \u0432\u0435\u043b\u0438\u043a](https://codeforces.com/problemset/problem/104820/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới có các hàng và cột có kích thước không đồng đều. Mỗi hàng có chiều cao dương được cho bởi mảng`A`và mỗi cột có chiều rộng dương được cho bởi mảng`B`. Nếu chúng ta lấy bất kỳ khối hàng liền kề nào và bất kỳ khối cột liền kề nào, chúng tạo thành một vùng hình chữ nhật được tạo thành từ các ô đơn vị, trong đó mỗi ô`(i, j)`tồn tại bên trong một hình chữ nhật hình học lớn hơn mà phần đóng góp diện tích của nó phụ thuộc vào các hàng và cột đã chọn. 

Nếu chúng ta chọn các hàng từ`l`ĐẾN`r`và các cột từ`x`ĐẾN`y`, hình được chọn là một ma trận con đầy đủ. Số lượng các khu vực được chọn chỉ đơn giản là số lượng ô trong ma trận con đó, bằng`(r - l + 1) * (y - x + 1)`. 

Tuy nhiên, có một hạn chế về mặt hình học. Mỗi hàng đóng góp một chiều cao, do đó tổng chiều cao của đoạn hàng được chọn là tổng của`A[i]`trên đoạn đó và mỗi cột đóng góp một chiều rộng, do đó tổng chiều rộng là tổng của`B[j]`trên đoạn cột. Diện tích vật lý của hình chữ nhật thu được là tích của hai tổng này và tích này không được vượt quá`S`. 

Nhiệm vụ là chọn một đoạn hàng liền kề và một đoạn cột liền kề để tối đa hóa số lượng ô trong hình chữ nhật thu được trong khi vẫn thỏa mãn giới hạn diện tích hình học. 

Những hạn chế`N, M ≤ 1000`ngụ ý rằng bất kỳ giải pháp nào gần như vượt xa`O(N^2 M)`hoặc`O(N M^2)`sẽ quá chậm. Một giải pháp xung quanh`O(N^2 log M + M^2 log N)`hoặc tốt hơn là có thể chấp nhận được, vì khoảng một triệu khoảng cho mỗi chiều là khả thi, nhưng các lần quét toàn bộ lồng nhau cho mỗi truy vấn thì không. 

Một nỗ lực ngây thơ thử từng cặp phân đoạn hàng và cột một cách độc lập sẽ kiểm tra đại khái`O(N^2 M^2)`sự kết hợp, đó là về`10^12`, vượt xa giới hạn. 

Một trường hợp thất bại tinh tế đối với việc cắt tỉa ngây thơ xuất hiện khi một chiều lớn nhưng lại làm giảm nhẹ phạm vi cho phép của chiều kia. Ví dụ: việc chọn một phân đoạn hàng lớn hơn một chút có thể làm giảm độ rộng cột cho phép chỉ bằng một cột, nhưng việc mất một cột đó có thể làm giảm đáng kể số lượng khu vực. Bất kỳ sự mở rộng tham lam nào trong một chiều mà không xem xét tất cả các khoảng sẽ bỏ lỡ những sự đánh đổi như vậy. 

## Phương pháp tiếp cận 

Một giải pháp lực lượng vũ phu trực tiếp liệt kê mọi khoảng thời gian hàng có thể và mọi khoảng thời gian cột có thể. Đối với mỗi cặp, chúng tôi tính tổng của`A`Và`B`trong những khoảng thời gian đó, hãy kiểm tra xem sản phẩm của họ có nhiều nhất`S`và tính số lượng ô. Điều này đúng vì nó kiểm tra toàn diện tất cả các hình chữ nhật hợp lệ. Vấn đề là chi phí: có`O(N^2)`khoảng cách hàng và`O(M^2)`khoảng thời gian của cột và việc kiểm tra từng cặp sẽ mất thời gian không đổi hoặc logarit nếu sử dụng tổng tiền tố, đưa ra khoảng`10^12`đánh giá trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là các lựa chọn hàng và cột là độc lập ngoại trừ thông qua một ràng buộc vô hướng duy nhất: tích của các tổng của chúng. Điều này gợi ý việc tách vấn đề thành hai giai đoạn. Nếu chúng ta cố định một khoảng hàng thì tổng của nó sẽ trở thành một hằng số`H`. Khi đó bất kỳ khoảng cột hợp lệ nào cũng phải thỏa mãn`sum(B[l..r]) ≤ S / H`. Đối với ngưỡng cố định đó, chúng tôi muốn khoảng thời gian cột có độ dài tối đa. 

Điều này biến vấn đề thành một mẫu: tính toán trước tất cả các khoảng cột một lần, lưu trữ chúng theo tổng của chúng và trả lời nhiều truy vấn “độ dài tối đa giữa các khoảng có tổng ≤ T”. Điều tương tự có thể được thực hiện đối xứng cho các hàng, nhưng thực hiện một lần là đủ. 

Chúng tôi giảm vấn đề lựa chọn 2D thành việc tạo ra tất cả các khoảng 1D cho một chiều và truy vấn chúng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 M2) | O(1) | Quá chậm | 
| Tiền xử lý khoảng thời gian + tìm kiếm nhị phân | O((N2 + M2) log M2) | O(M²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước tất cả các khoảng cột và nén thông tin của chúng vào một cấu trúc cho phép truy vấn nhanh theo ràng buộc tổng. 

1. Tính tổng tiền tố cho mảng`B`, do đó, bất kỳ tổng khoảng nào cũng có thể thu được trong O(1). Điều này cho phép chúng tôi đánh giá mọi phân đoạn cột một cách hiệu quả. 
2. Liệt kê tất cả các khoảng cột`(l, r)`và tính hai giá trị cho mỗi giá trị:`sumB = B[l] + ... + B[r]`Và`lenB = r - l + 1`. Điều này tạo ra khoảng`M² / 2`khoảng thời gian. 
3. Sắp xếp các khoảng này theo`sumB`. Sau khi sắp xếp, chúng tôi xây dựng một mảng trong đó tại mỗi vị trí chúng tôi lưu trữ tối đa`lenB`thấy cho đến nay. Điều này biến cấu trúc thành một công cụ truy vấn đơn điệu: cho bất kỳ ngưỡng nào`T`, chúng ta có thể tìm thấy độ dài cột tốt nhất trong số tất cả các khoảng có tổng ≤`T`sử dụng tìm kiếm nhị phân theo sau là tra cứu tối đa tiền tố. 
4. Lặp lại việc chuẩn bị tổng tiền tố tương tự cho`A`. 
5. Liệt kê tất cả các khoảng hàng`(i, j)`, tính toán`sumA = A[i] + ... + A[j]`Và`lenA = j - i + 1`. 
6. Với mỗi khoảng thời gian của hàng, hãy tính tổng cột tối đa được phép`T = S // sumA`. 
7. Truy vấn cấu trúc cột được xử lý trước để có được độ dài cột tốt nhất có thể mà tổng không vượt quá`T`. 
8. Cập nhật câu trả lời bằng`lenA * bestLenB`. 

Lý do điều này có hiệu quả là vì khi khoảng cách hàng được cố định, việc lựa chọn cột sẽ trở thành một vấn đề tối ưu hóa ràng buộc độc lập đối với tập hợp được tính toán trước và ngược lại. 

### Tại sao nó hoạt động 

Đối với mỗi hình chữ nhật hợp lệ, tồn tại chính xác một khoảng hàng và một khoảng cột biểu thị nó. Thuật toán xem xét từng khoảng thời gian một cách rõ ràng và với mỗi khoảng thời gian như vậy, thuật toán sẽ tính toán khoảng thời gian cột tương thích tốt nhất theo ràng buộc chính xác do lựa chọn hàng đó gây ra. Vì các khoảng cột được liệt kê đầy đủ và tối ưu hóa cho mọi ngưỡng tổng có thể có nên không có cấu hình khả thi nào bị bỏ qua. Việc phân rã duy trì tính tối ưu vì ràng buộc chỉ ghép các kích thước thông qua một tích vô hướng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_intervals(arr):
    n = len(arr)
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + arr[i]

    intervals = []
    for i in range(n):
        for j in range(i, n):
            s = pref[j + 1] - pref[i]
            length = j - i + 1
            intervals.append((s, length))
    return intervals

def build_query_structure(intervals):
    intervals.sort()
    best = []
    max_len = 0

    for s, l in intervals:
        if l > max_len:
            max_len = l
        best.append((s, max_len))
    return best

def query(best, T):
    import bisect
    idx = bisect.bisect_right(best, (T, 10**18)) - 1
    if idx < 0:
        return 0
    return best[idx][1]

def solve():
    N, M, S = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    col_intervals = build_intervals(B)
    col_best = build_query_structure(col_intervals)

    row_intervals = build_intervals(A)

    ans = 0

    for sumA, lenA in row_intervals:
        if sumA > 0:
            T = S // sumA
            lenB = query(col_best, T)
            ans = max(ans, lenA * lenB)

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi từng mảng thành tất cả các phân đoạn liền kề có thể bằng cách sử dụng tổng tiền tố. Mỗi đoạn được biểu diễn bằng tổng và độ dài của nó. Đối với các cột, các phân đoạn này được sắp xếp theo tổng và được nén để với bất kỳ ngưỡng tổng nào, chúng tôi có thể truy xuất chiều rộng tối đa có thể đạt được một cách hiệu quả. 

Hàm truy vấn sử dụng tìm kiếm nhị phân trên cấu trúc nén. Vì cấu trúc tổng thể là đơn điệu nên độ dài tốt nhất cho bất kỳ ngưỡng nào luôn được tìm thấy ở hoặc trước khoảng cuối cùng có tổng nằm trong giới hạn. 

Vòng lặp hàng đánh giá từng lựa chọn chiều cao có thể có và chuyển nó thành tổng cột tối đa cho phép. Tích của cột phù hợp nhất và độ dài hàng sẽ đưa ra câu trả lời cho thí sinh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
A = [2, 4, 1, 3]
B = [4, 2, 1, 2]
S = 2
```Đầu tiên chúng tôi liệt kê một số khoảng cột: 

| khoảng thời gian | tổngB | lenB | 
| --- | --- | --- | 
| [1] | 4 | 1 | 
| [2] | 2 | 1 | 
| [3] | 1 | 1 | 
| [4] | 2 | 1 | 
| [2,3] | 3 | 2 | 
| [3,4] | 3 | 2 | 

Sau khi nén, đối với các ngưỡng nhỏ như`T = 1`, chỉ những khoảng có tổng ≤ 1 mới có thể sử dụng được, cho độ dài 1 tốt nhất. 

Khoảng thời gian hàng bao gồm các hàng phần tử đơn như`[3]`với sumA = 1 và lenA = 1. Điều này mang lại`T = 2`, vì vậy chúng ta có thể sử dụng bất kỳ khoảng cột nào có tổng 2, cho độ dài cột tốt nhất là 1. Tích số là 1. 

Việc thử tính tổng số hàng lớn hơn sẽ giảm ngay lập tức`T`đến 0, không có cột hợp lệ. Câu trả lời hay nhất vẫn là 1. 

Dấu vết này cho thấy các ràng buộc chặt chẽ buộc giải pháp hướng tới các hình chữ nhật hợp lệ tối thiểu. 

### Mẫu 2 

đầu vào:```
A = [2, 4, 1, 3]
B = [4, 2, 1, 2]
S = 20
```Bây giờ các ràng buộc đã đủ lỏng lẻo để cho phép các hình chữ nhật lớn hơn. 

Đối với khoảng thời gian hàng`[4, 1, 3]`, sumA = 8 và lenA = 3, vậy`T = 20 // 8 = 2`. Chúng ta có thể chọn khoảng cột tốt nhất với tổng 2, chẳng hạn như`[2]`hoặc`[3]`hoặc`[4]`, cho lenB = 1. Tích là 3. 

Đối với khoảng thời gian hàng`[1, 3]`, sumA = 4 và lenA = 2, vậy`T = 5`. Khoảng cách cột bây giờ`[2,3]`hợp lệ với tổng 3 và độ dài 2, cho kết quả 4. 

Sự kết hợp tốt nhất đạt được khi cả hai kích thước đều lớn vừa phải trong khi vẫn tôn trọng ràng buộc của sản phẩm. 

Dấu vết này nêu bật cách tăng diện tích cho phép sẽ mở rộng các lựa chọn cột khả thi một cách phi tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 + M2 log M2) | Tất cả các khoảng được tạo theo thời gian bậc hai, các khoảng cột sắp xếp chiếm ưu thế với hệ số log | 
| Không gian | O(M²) | Lưu trữ tất cả các khoảng cột và cấu trúc nén | 

Quá trình tiền xử lý bậc hai có thể chấp nhận được vì cả hai chiều đều được giới hạn ở mức 1000, tạo ra khoảng một triệu khoảng cho mỗi mảng. Truy vấn logarit trên mỗi khoảng thời gian hàng giữ tổng số hoạt động trong khoảng vài chục triệu, phù hợp thoải mái trong giới hạn điển hình cho Python theo các ràng buộc của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    N, M, S = map(int, sys.stdin.readline().split())
    A = list(map(int, sys.stdin.readline().split()))
    B = list(map(int, sys.stdin.readline().split()))

    def build(arr):
        n = len(arr)
        pref = [0]*(n+1)
        for i in range(n):
            pref[i+1]=pref[i]+arr[i]
        res=[]
        for i in range(n):
            for j in range(i,n):
                res.append((pref[j+1]-pref[i], j-i+1))
        return res

    def build_best(intervals):
        intervals.sort()
        best=[]
        mx=0
        for s,l in intervals:
            mx=max(mx,l)
            best.append((s,mx))
        return best

    def query(best,T):
        import bisect
        i=bisect.bisect_right(best,(T,10**18))-1
        return 0 if i<0 else best[i][1]

    col=build(B)
    cb=build_best(col)
    row=build(A)

    ans=0
    for sA,lA in row:
        if sA<=0: 
            continue
        T=S//sA
        ans=max(ans,lA*query(cb,T))
    return str(ans)

# provided samples
assert run("4 4 2\n2 4 1 3\n4 2 1 2\n") == "1"
assert run("4 4 20\n2 4 1 3\n4 2 1 2\n") == "6"

# custom cases
assert run("1 1 100\n5\n5\n") == "1", "single cell"
assert run("3 3 1\n1 1 1\n1 1 1\n") == "1", "tight constraint"
assert run("3 3 1000\n1 2 3\n1 2 3\n") == "9", "full rectangle possible"
assert run("2 3 5\n2 2\n1 2 1\n") >= "1", "basic feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | xử lý cấu trúc tối thiểu | 
| ràng buộc chặt chẽ | 1 | cực hạn S | 
| hình chữ nhật đầy đủ có thể | 9 | trường hợp mở rộng tối đa | 
| tính khả thi cơ bản | ≥1 | giá trị không tầm thường | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tổng số hàng vượt quá`S`. Trong hoàn cảnh đó, mọi`T`trở thành 0 và không có khoảng cột đủ điều kiện. Thuật toán xử lý việc này vì tìm kiếm nhị phân trả về`0`và bản cập nhật sản phẩm không bao giờ được kích hoạt. 

Một trường hợp khác xuất hiện khi chỉ có khoảng thời gian một hàng là hợp lệ. Ví dụ, nếu`A = [100, 1, 100]`Và`S = 10`, chỉ hàng giữa mới đóng góp bất kỳ cấu hình hợp lệ nào. Vòng lặp tự nhiên đánh giá từng khoảng thời gian một cách độc lập và chỉ khoảng thời gian có`sumA = 1`tạo ra kết quả cột khác 0. 

Trường hợp thứ ba liên quan đến sự phân bố không đồng đều trong đó một đoạn hàng rất nhỏ mở ra một đoạn cột lớn không cân xứng. Bởi vì tất cả các khoảng hàng được liệt kê rõ ràng thay vì mở rộng một cách tham lam, thuật toán không bỏ sót các cấu trúc tối ưu bất đối xứng này.
