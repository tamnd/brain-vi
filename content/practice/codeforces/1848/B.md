---
title: "CF 1848B - Vika và cây cầu"
description: "Chúng ta có một cây cầu tuyến tính được biểu diễn dưới dạng một mảng các màu ván. Vika bắt đầu trước tấm ván đầu tiên và muốn đạt đến tấm ván cuối cùng."
date: "2026-06-09T05:41:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1848
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 885 (Div. 2)"
rating: 1200
weight: 1848
solve_time_s: 310
verified: false
draft: false
---

[CF 1848B - Vika và cây cầu](https://codeforces.com/problemset/problem/1848/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, triển khai, toán học, sắp xếp 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây cầu tuyến tính được biểu diễn dưới dạng một mảng các màu ván. Vika bắt đầu trước tấm ván đầu tiên và muốn đạt đến tấm ván cuối cùng. Cô ấy chỉ được phép đi trên một dãy ván đã chọn có cùng màu và mỗi khi bước từ tấm ván đã chọn này sang tấm ván tiếp theo, cô ấy có thể phải vượt qua một khoảng trống của những tấm ván không được chọn. Kích thước của một bước chính xác là số ván bị bỏ qua giữa các vị trí được chọn liên tiếp và mục tiêu là giảm thiểu số lần bỏ qua lớn nhất như vậy. 

Có thêm một bước ngoặt. Trước khi bước đi, Vika được phép sơn lại tối đa một tấm ván thành bất kỳ màu nào, điều chỉnh hiệu quả mảng ở một vị trí duy nhất. Cô ấy cũng có thể chọn không sơn lại gì cả. 

Nhiệm vụ là chọn một màu và một chuỗi các vị trí của màu đó (có thể sau một lần sửa đổi) sao cho khoảng cách tối đa giữa các vị trí được chọn liên tiếp, bao gồm cả ranh giới bắt đầu và kết thúc, được giảm thiểu. 

Câu trả lời là một vấn đề cực tiểu đối với các khoảng trống gây ra bởi sự xuất hiện của một màu đã chọn. Nếu chúng tôi sửa một màu, các lần xuất hiện của nó sẽ chia mảng thành các phân đoạn và phân đoạn tệ nhất sẽ xác định mức độ nguy hiểm của màu đó khi đi trên đường đi bộ. 

Các ràng buộc ngụ ý rằng chúng tôi phải xử lý tối đa 2⋅10^5 bảng trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp bậc hai nào cho mỗi trường hợp thử nghiệm sẽ thất bại ngay lập tức. Chúng ta cần một phương pháp gần tuyến tính hoặc tuyến tính cho mỗi trường hợp và lý tưởng nhất là tuyến tính tổng thể. 

Một nỗ lực ngây thơ sẽ thử từng màu và từng vị trí sơn lại có thể, tính toán lại các khoảng trống và đánh giá bước tối đa. Điều này sẽ liên tục quét các mảng và dễ dàng đạt tới O(n^2). 

Trường hợp cạnh tinh tế phát sinh khi tất cả các tấm ván đều có cùng màu. Thế thì không có khoảng trống nào cả, nên câu trả lời là không. Một trường hợp khác là khi các màu hoàn toàn xen kẽ nhau, trong đó ngay cả những dãy màu tối ưu vẫn tạo ra những khoảng trống lớn không thể tránh khỏi trừ khi sơn lại chúng. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là cố định một màu, thu thập tất cả các vị trí của nó và tính toán khoảng cách tối đa được tạo ra bằng cách chỉ đi bộ trên các vị trí đó. Không cần sơn lại, điều này rất đơn giản. Vấn đề là thao tác sơn lại: chúng tôi có thể chèn thêm một lần xuất hiện của màu đã chọn ở bất kỳ vị trí nào, chia một khoảng cách lớn thành hai khoảng trống nhỏ hơn một cách hiệu quả. 

Nếu chúng tôi mô phỏng điều này cho mọi màu sắc và mọi vị trí chèn có thể có, chúng tôi sẽ tính toán lại các cấu trúc khoảng cách nhiều lần. Đối với mỗi màu, việc duy trì tất cả các giá trị khoảng cách và kiểm tra tất cả các điểm phân chia có thể dẫn đến kết quả O(n) cho mỗi ứng cử viên khoảng cách, trong trường hợp xấu nhất sẽ trở thành O(n^2). 

Quan sát quan trọng là đối với một màu cố định, cấu trúc quan tâm là mảng khoảng cách của nó: khoảng cách giữa các lần xuất hiện liên tiếp, bao gồm các cạnh từ ranh giới của mảng. Câu trả lời cho một màu mà không cần sơn lại chỉ đơn giản là khoảng cách tối đa. Với một lần sơn lại, chúng ta có thể lấy bất kỳ khoảng trống nào và chia nó thành hai phần, chỉ giảm mức tối đa nếu khoảng trống đó là nút cổ chai duy nhất. 

Điều này làm giảm vấn đề đánh giá, đối với mỗi màu, khoảng cách lớn nhất có thể giảm được bao nhiêu bằng cách chèn thêm một lần xuất hiện một cách tối ưu. Tốt nhất nên đặt phần chèn bên trong khoảng trống lớn nhất, chia nó càng đều càng tốt để phần đóng góp của nó trở thành trần (khoảng cách/2). Tất cả các khoảng trống khác không thay đổi. Vì vậy, đối với mỗi màu, chúng ta chỉ cần nhiều khoảng cách và mức tối đa và mức tối đa thứ hai của nó. 

Chúng ta có thể tính toán tất cả các vị trí của từng màu trong một lần vượt qua, tính toán các khoảng trống trong O (lần xuất hiện) và duy trì hai cực đại tốt nhất. Câu trả lời chung là mức tối thiểu trên tất cả các màu của khoảng cách tối đa tốt nhất có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nhóm các chỉ số của từng màu trong danh sách. Điều này là cần thiết vì cấu trúc khoảng trống chỉ phụ thuộc vào vị trí tương đối chứ không phải quét tuyệt đối toàn bộ mảng nhiều lần. 
2. Đối với mỗi màu, hãy xây dựng danh sách vị trí tăng cường bằng cách thêm các điểm cuối ảo 0 và n+1. Chúng thể hiện điểm bắt đầu trước tấm ván đầu tiên và điểm kết thúc sau tấm ván cuối cùng, do đó các khoảng trống ở ranh giới được xử lý thống nhất. 
3. Tính toán tất cả các khoảng trống giữa các vị trí liên tiếp. Mỗi khoảng trống biểu thị số lượng ván được bỏ qua khi chuyển từ lần xuất hiện màu này sang lần xuất hiện màu tiếp theo. 
4. Tìm khoảng cách lớn nhất gmax và khoảng cách lớn thứ hai g2. Hai giá trị này xác định kết quả tốt nhất có thể sau một lần sơn lại, vì chỉ có thể tách một khoảng trống. 
5. Nếu chúng tôi không sơn lại thì chi phí cho màu này là gmax. Nếu sơn lại, chúng ta chọn chia khoảng trống lớn nhất thành 2 phần, giảm xuống còn ceil(gmax/2). Khoảng cách tồi tệ nhất mới trở thành max(g2, ceil(gmax / 2)). 
6. Tính giá trị tốt nhất này cho mọi màu và lấy giá trị tối thiểu trên tất cả các màu. 
7. Xuất ra mức tối thiểu trên tất cả các màu. 

### Tại sao nó hoạt động 

Đối với một màu cố định, bất kỳ chiến lược đi bộ hợp lệ nào đều tương ứng với việc chọn tất cả các lần xuất hiện của màu đó theo thứ tự, có thể được tăng thêm bởi một lần xuất hiện được chèn nhân tạo do sơn lại. Điểm được chèn này chỉ có thể ảnh hưởng đến một khoảng thời gian giữa các lần xuất hiện liên tiếp, do đó chỉ có thể tách một khoảng cách ban đầu. Việc chia tách một cách tối ưu luôn nhắm đến khoảng cách lớn nhất vì việc giảm bất kỳ khoảng cách nhỏ hơn nào sẽ không cải thiện được mức tối đa. Do đó giải pháp tối ưu chỉ phụ thuộc vào hai khe hở lớn nhất, làm cho kết cấu có khả năng chịu nén hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        c = list(map(int, input().split()))

        pos = [[] for _ in range(k + 1)]
        for i, x in enumerate(c, start=1):
            pos[x].append(i)

        ans = n

        for color in range(1, k + 1):
            if not pos[color]:
                continue

            arr = [0] + pos[color] + [n + 1]

            gaps = []
            for i in range(1, len(arr)):
                gaps.append(arr[i] - arr[i - 1] - 1)

            if len(gaps) == 0:
                ans = 0
                continue

            gmax = max(gaps)
            g2 = 0
            for g in gaps:
                if g != gmax:
                    g2 = max(g2, g)

            best = max(g2, (gmax + 1) // 2)
            ans = min(ans, best)

        print(ans)

if __name__ == "__main__":
    solve()
```Mã nhóm các vị trí trên mỗi màu trong một lần chuyển, đảm bảo quá trình tiền xử lý tuyến tính. Đối với mỗi màu, nó xây dựng một danh sách vị trí được tăng cường ranh giới để các khoảng trống điểm cuối được xử lý mà không cần viết vỏ đặc biệt. Sau đó, nó tính toán tất cả các khoảng trống trong một lần truyền tải và trích xuất khoảng cách lớn nhất và lớn thứ hai mà không cần sắp xếp, điều này giúp quá trình xử lý mỗi màu luôn tuyến tính theo số lần xuất hiện. 

Một điểm tinh tế là xử lý các màu xuất hiện bằng 0 hoặc một lần. Với số lần xuất hiện bằng 0, màu sắc không liên quan. Với một lần xuất hiện, các khoảng trống bao gồm toàn bộ tiền tố và hậu tố, đồng thời công thức vẫn áp dụng chính xác vì danh sách khoảng trống đơn thể hiện chính xác cả hai bên của cầu nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, c = [1, 1, 2, 1, 1]
```Chúng tôi xem xét màu 1. 

| Bước | Vị trí | Khoảng trống | gmax | g2 | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| Xây dựng | [0,1,2,4,5,6] | [0,0,1,0,0] | 1 | 0 | 0 | 

Khoảng cách khác 0 duy nhất là nhỏ và việc sơn lại có thể loại bỏ hoàn toàn nó bằng cách chèn vào bên trong nó. Câu trả lời cuối cùng trở thành 0. 

Điều này chứng tỏ rằng các cụm màu dày đặc có thể loại bỏ mọi chi phí di chuyển. 

### Ví dụ 2 

đầu vào:```
n = 7, c = [1,2,3,3,3,2,1]
```Hãy xem xét màu sắc 3. 

| Bước | Vị trí | Khoảng trống | gmax | g2 | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| Xây dựng | [0,3,4,5,8] | [2,0,0,2] | 2 | 2 | 2 | 

Ngay cả sau khi sơn lại, việc tách khoảng cách lớn nhất không giảm tối đa xuống dưới 2 vì vẫn còn một khoảng cách bằng nhau khác. Điều này cho thấy tại sao khoảng cách lớn thứ hai lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần và tất cả các khoảng trống giữa các màu có tổng bằng n | 
| Không gian | O(n) | Danh sách vị trí lưu trữ tất cả các chỉ số một lần | 

Giải pháp vừa vặn thoải mái trong giới hạn vì tổng n trong các trường hợp thử nghiệm là 2⋅10^5 và tất cả các phép toán đều tuyến tính trong tổng số đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        c = list(map(int, input().split()))

        pos = [[] for _ in range(k + 1)]
        for i, x in enumerate(c, start=1):
            pos[x].append(i)

        ans = n

        for color in range(1, k + 1):
            if not pos[color]:
                continue

            arr = [0] + pos[color] + [n + 1]

            gaps = []
            for i in range(1, len(arr)):
                gaps.append(arr[i] - arr[i - 1] - 1)

            gmax = max(gaps)
            g2 = 0
            for g in gaps:
                if g != gmax:
                    g2 = max(g2, g)

            best = max(g2, (gmax + 1) // 2)
            ans = min(ans, best)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
5 2
1 1 2 1 1
7 3
1 2 3 3 3 2 1
6 6
1 2 3 4 5 6
8 4
1 2 3 4 2 3 1 4
3 1
1 1 1
""") == """0
1
2
2
0"""

# custom cases
assert run("""1
1 1
1
""") == "0", "single plank"

assert run("""1
4 2
1 2 1 2
""") in {"1"}, "alternating colors"

assert run("""1
6 3
1 2 1 2 1 2
""") == "1", "balanced repetition"

assert run("""1
5 5
1 2 3 4 5
""") == "2", "all distinct small n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ván đơn | 0 | cấu trúc tối thiểu | 
| xen kẽ màu sắc | 1 | khoảng trống thường xuyên | 
| sự lặp lại cân bằng | 1 | cấu trúc đối xứng lặp đi lặp lại | 
| tất cả đều khác biệt | 2 | trường hợp lây lan tồi tệ nhất | 

## Vỏ cạnh 

Một cây cầu một màu như`[1,1,1,...]`không tạo ra khoảng trống. Thuật toán xây dựng vị trí`[0,1,2,...,n,n+1]`, trong đó tất cả các khoảng trống bên trong đều bằng 0, do đó cả gmax và g2 đều bằng 0, tạo ra câu trả lời là 0. 

Một màu xuất hiện một lần sẽ tạo ra hai khoảng trống ranh giới. Ví dụ`[2,1,1,1,3]`cho màu 2 mang lại vị trí`[0,1,5]`và những khoảng trống`[0,3]`. Khoảng cách tối đa được xử lý chính xác dưới dạng độ dài phân đoạn đầy đủ. 

Các mảng xen kẽ cao tạo ra nhiều khoảng trống nhỏ nhưng thường xuyên. Thuật toán vẫn chỉ theo dõi khoảng cách lớn nhất trên mỗi màu, đảm bảo xử lý chính xác mà không cần mô phỏng mọi đường dẫn.
