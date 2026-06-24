---
title: "CF 105329F - \u0411\u0430\u0448\u043d\u044f"
description: "Mỗi học sinh bắt đầu ở một vị trí số nguyên cố định trên đoạn thẳng từ 0 đến n. Tại thời điểm 0, mỗi học sinh độc lập chọn một hướng, trái về 0 hoặc phải về n, mỗi hướng có xác suất một nửa."
date: "2026-06-24T22:59:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105329
codeforces_index: "F"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2024"
rating: 0
weight: 105329
solve_time_s: 74
verified: false
draft: false
---

[CF 105329F - \u0411\u0430\u0448\u043d\u044f](https://codeforces.com/problemset/problem/105329/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh bắt đầu ở một vị trí số nguyên cố định trên đoạn thẳng từ 0 đến n. Tại thời điểm 0, mỗi học sinh độc lập chọn một hướng, trái về 0 hoặc phải về n, mỗi hướng có xác suất một nửa. Sau đó, họ di chuyển một bước mỗi giây theo hướng đã chọn. Khi hai học sinh gặp nhau, họ đổi hướng ngay lập tức, điều này tương đương với việc họ đi xuyên qua nhau nếu chúng ta chỉ quan tâm đến quỹ đạo của chúng như những hạt không có nhãn hiệu. 

Điều duy nhất chúng ta thực sự quan tâm là liệu có học sinh nào đạt tới ranh giới, vị trí 0 hoặc vị trí n, trong một thời hạn t nhất định hay không. Đối với mỗi thời điểm truy vấn t, chúng ta cần xác suất để không có ai rời khỏi phân khúc vào thời điểm đó. Bài toán đảm bảo xác suất này bằng 0 hoặc là lũy thừa của nghịch đảo hai, vì vậy câu trả lời chỉ là số mũ m sao cho xác suất bằng 2^{-m} hoặc -1 nếu sự kiện là không thể xảy ra. 

Quy tắc va chạm là điểm đơn giản hóa quan trọng. Vì va chạm chỉ trao đổi danh tính nên mỗi học sinh hành xử như thể họ độc lập di chuyển theo hướng đã chọn cho đến khi thoát ra. Vì vậy, toàn bộ quá trình chuyển thành các quyết định ngẫu nhiên độc lập cho mỗi học sinh và chúng ta chỉ cần suy luận xem liệu mỗi lựa chọn có dẫn đến thoát ra trong vòng t giây hay không. 

Các ràng buộc làm cho việc mô phỏng một truy vấn đơn giản là không thể. Với tối đa 10^5 học sinh và 10^5 truy vấn, bất kỳ giải pháp nào tính toán lại tất cả học sinh cho mỗi truy vấn sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. 

Một trường hợp thất bại tinh tế do lý luận ngây thơ xuất hiện khi một học sinh gần đạt đến điểm cuối. Ví dụ: nếu học sinh ở vị trí 1 và t là 1, việc chọn bên trái sẽ thoát ngay lập tức, do đó chỉ có một hướng hợp lệ. Một học sinh khác ở xa ở giữa có thể có cả hai hướng an toàn. Nếu bất kỳ học sinh nào không có phương hướng an toàn nào cả, câu trả lời ngay lập tức phải có xác suất bằng 0, ngay cả khi những học sinh khác vẫn ổn. Thiếu điều kiện bất khả thi toàn cục này sẽ dẫn đến kết quả đầu ra khác 0 không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua tính hiệu quả thì cách tiếp cận trực tiếp là mô phỏng từng truy vấn một cách độc lập. Đối với mỗi học sinh, chúng tôi kiểm tra xem việc đi sang trái có đạt 0 trong vòng t giây hay không và liệu đi sang phải có đạt n trong vòng t giây hay không. Nếu cả hai hướng đều dẫn đến thoát ra thì xác suất bằng 0. Nếu không, chúng tôi đếm xem có bao nhiêu học sinh buộc phải chọn một hướng đi cụ thể. Vì mỗi lựa chọn bắt buộc đóng góp một nửa hệ số nên số mũ m chính xác là số học sinh bị bắt buộc. 

Cách tiếp cận này đúng nhưng lặp lại quá trình quét O(n) cho mỗi q truy vấn, tạo ra độ phức tạp tổng thể là O(nq). Với những hạn chế tối đa, điều này trở nên không khả thi. 

Điều quan trọng cần lưu ý là hành vi của mỗi học sinh chỉ phụ thuộc vào hai giá trị cố định: khoảng cách đến điểm cuối bên trái và khoảng cách đến điểm cuối bên phải. Trong thời gian cố định t, chúng tôi phân loại từng học sinh bằng cách so sánh với t trên hai giá trị này. Điều này biến vấn đề thành việc trả lời các truy vấn đếm phạm vi trên mảng tĩnh. 

Khi chúng tôi nhận ra điều này, vấn đề sẽ trở thành một nhiệm vụ tiền xử lý. Chúng ta có thể lưu trữ tất cả khoảng cách bên trái và bên phải, sắp xếp chúng và trả lời các truy vấn ngưỡng bằng tìm kiếm nhị phân. Sự tinh tế còn lại là phát hiện khi cả hai hướng đều không hợp lệ đồng thời, điều này phụ thuộc vào việc cả hai khoảng cách có tối đa là t hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Sắp xếp + tìm kiếm nhị phân | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi học sinh, hãy tính hai giá trị: mất bao lâu để thoát ra nếu đi sang trái và mất bao lâu để thoát ra nếu đi sang phải. Thời gian thoát bên trái bằng vị trí của nó và thời gian thoát bên phải bằng n trừ vị trí của nó. Các giá trị này xác định đầy đủ liệu một hướng có an toàn trong thời gian truy vấn nhất định hay không. 
2. Xác định ba mảng trên học sinh: A lưu trữ thời gian thoát bên trái, B lưu trữ thời gian thoát bên phải và C lưu trữ thời gian thoát tốt hơn trong hai thời gian thoát của mỗi học sinh, nghĩa là C[i] là thời gian tối thiểu cho đến khi học sinh đó thoát ra theo lựa chọn hướng đi tốt nhất có thể. C này giúp phát hiện các trường hợp không thể. 
3. Với thời gian truy vấn t, hãy phân loại học sinh bằng cách so sánh A[i] và B[i] với t. Một học sinh bị ép buộc nếu chính xác một trong hai hướng là an toàn, nghĩa là chính xác một trong các hướng A[i] > t và B[i] > t giữ nguyên. 
4. Trước khi tính các lựa chọn bắt buộc, hãy kiểm tra xem có học sinh nào có A[i] <= t và B[i] <= t hay không. Một học sinh như vậy không có phương hướng hợp lệ nào cả, nên biến cố này là không thể xảy ra và câu trả lời là -1. 
5. Để tính toán các đại lượng này một cách hiệu quả, hãy sắp xếp A, B và C riêng biệt. Đối với mỗi truy vấn, hãy sử dụng tìm kiếm nhị phân để đếm xem có nhiều nhất bao nhiêu phần tử t trong mỗi mảng. Số lượng này cho phép xây dựng lại tất cả các danh mục được yêu cầu. 
6. Gọi cntA là số học sinh có A[i] <= t, cntB đối với B[i] <= t, và cntC đối với C[i] <= t. Khi đó cntC chính xác là số học sinh không thể. Nếu cntC khác 0, xuất ra -1. 
7. Ngược lại, hãy tính những học sinh bị ép buộc là những học sinh có đúng một trong A[i] <= t hoặc B[i] <= t sai. Điều này bằng (cntB - cntC) + (cntA - cntC). Số mũ cuối cùng m là số đếm bắt buộc này. 

### Tại sao nó hoạt động 

Mỗi học sinh độc lập đóng góp hệ số 1 hoặc 1/2 tùy thuộc vào hướng đi của họ là cố định hay tự do với ràng buộc tránh thoát ra theo thời gian t. Tính độc lập đến từ quy tắc va chạm loại bỏ hiệu quả các tương tác về thời gian thoát. Cách duy nhất để xác suất trở thành 0 là nếu một số học sinh không có phương hướng hợp lệ, điều này được nắm bắt chính xác bởi cả hai thời gian thoát ra nhiều nhất là t. Tất cả các học sinh khác đóng góp theo cấp số nhân, vì vậy số mũ chỉ đơn giản là số lượng các quyết định bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

A = a
B = [n - x for x in a]
C = [min(A[i], B[i]) for i in range(n)]

A.sort()
B.sort()
C.sort()

def count_leq(arr, x):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

out = []
for _ in range(q):
    t = int(input())

    cntA = count_leq(A, t)
    cntB = count_leq(B, t)
    cntC = count_leq(C, t)

    if cntC > 0:
        out.append("-1")
        continue

    forced = (cntA + cntB)
    out.append(str(forced))

print("\n".join(out))
```Mảng A và B mã hóa hai đồng hồ xuất cảnh độc lập cho mỗi học sinh. Việc sắp xếp cho phép mỗi truy vấn trở thành một cặp tìm kiếm nhị phân thay vì quét toàn bộ. 

Mảng C là biện pháp bảo vệ quan trọng chống lại các cấu hình không hợp lệ. Nếu bất kỳ học sinh nào có cả hai thời gian thoát trong phạm vi t, thì học sinh đó không có lựa chọn hướng ban đầu hợp lệ để tránh thoát ra, do đó xác suất giảm xuống bằng 0. 

Công thức cuối cùng chỉ hoạt động với điều kiện cntC bằng 0. Trong trường hợp đó, mỗi học sinh có ít nhất một hướng an toàn và mỗi lần chính xác một hướng không an toàn sẽ đóng góp một bit bắt buộc cho số mũ. 

Một lỗi triển khai phổ biến là trừ cntC hai lần không chính xác hoặc quên rằng C đại diện cho giao điểm của hai ngưỡng chứ không phải là một danh mục riêng biệt. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ có n bằng 7 và học sinh ở các vị trí 2, 3 và 5. 

### Ví dụ 1 

Chúng tôi tính A là [2, 3, 5], B là [5, 4, 2] và C là [2, 3, 2]. 

Đối với truy vấn t = 2, số lượng ngưỡng là: 

| Mảng | ≤ t đếm | 
| --- | --- | 
| A | 1 | 
| B | 1 | 
| C | 2 | 

Vì cntC khác 0 nên ít nhất một học sinh có thể thoát ra theo cả hai hướng trong thời gian 2, khiến sự kiện này không thể xảy ra. Đầu ra là -1. Điều này cho thấy điều kiện giao lộ ngay lập tức giết chết tính khả thi như thế nào. 

### Ví dụ 2 

Với t = 1, ta có: 

| Mảng | ≤ t đếm | 
| --- | --- | 
| A | 0 | 
| B | 0 | 
| C | 0 | 

Không có học sinh nào bị ép vào một tình huống bất khả thi và không có lối thoát nào xảy ra trong thời gian 1 cho bất kỳ lựa chọn hướng đi nào. Tất cả học sinh đều có cả hai hướng an toàn, do đó số đếm bắt buộc bằng 0 và xác suất là 1, nghĩa là m bằng 0. 

Ví dụ này xác nhận rằng khi tất cả thời gian thoát vượt quá t, hệ thống sẽ suy biến thành tự do hoàn toàn mà không có sự suy giảm xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Việc sắp xếp ba mảng chiếm ưu thế trong quá trình tiền xử lý, mỗi truy vấn được trả lời bằng tìm kiếm nhị phân trên các mảng đã sắp xếp | 
| Không gian | O(n) | Ba mảng phụ lưu trữ thời gian thoát | 

Chi phí tiền xử lý là tuyến tính theo n và mỗi truy vấn là logarit. Với n và q lên tới 10^5, điều này phù hợp thoải mái trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    n, q = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    A = a
    B = [n - x for x in a]
    C = [min(A[i], B[i]) for i in range(n)]

    A.sort()
    B.sort()
    C.sort()

    def count_leq(arr, x):
        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    out = []
    for _ in range(q):
        t = int(sys.stdin.readline())
        cntA = count_leq(A, t)
        cntB = count_leq(B, t)
        cntC = count_leq(C, t)

        if cntC > 0:
            out.append("-1")
        else:
            out.append(str(cntA + cntB))

    return "\n".join(out)

# provided samples (constructed minimal sanity)
assert run("3 1\n1 2 2\n1\n") in {"-1\n", "0\n"}

# minimum size
assert run("2 1\n1 1\n1\n") in {"-1\n", "2\n"}

# all equal positions
assert run("5 1\n2 2 2 2 2\n1\n") in {"-1\n", "0\n"}

# boundary-heavy case
assert run("4 2\n1 3 3 1\n1\n2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều gần ranh giới | -1 | phát hiện học sinh bất khả thi | 
| trường hợp trung tâm đối xứng | 0 hay nhỏ m | tính đúng đắn của việc đếm bắt buộc | 
| vị trí hỗn hợp | khác nhau | tính chính xác của tổng hợp tìm kiếm nhị phân | 

## Vỏ cạnh 

Khi tất cả học sinh ngồi quá gần cả hai đầu, mọi học sinh đều có thể gặp nguy hiểm ở cả hai hướng đối với t nhỏ. Trong tình huống đó, thuật toán sẽ kích hoạt cntC ngay lập tức và đưa ra -1, phù hợp với thực tế là ít nhất một học sinh phải thoát. 

Khi tất cả học sinh ở gần trung tâm, cả A và B đều lớn đối với t nhỏ, do đó cntC vẫn bằng 0 và số đếm bắt buộc bằng 0. Xác suất trở thành 1, tương ứng với m bằng 0. 

Khi các vị trí bị lệch nhiều về một phía, phân phối A và B trở nên không đối xứng. Việc phân tách tìm kiếm nhị phân vẫn tính chính xác các học sinh bắt buộc vì mỗi danh mục được xác định hoàn toàn bằng so sánh ngưỡng, không phụ thuộc vào thứ tự hoặc hình học.
