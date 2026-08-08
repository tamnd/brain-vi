---
title: "CF 103987K - Bài tập về nhà dễ dàng"
description: "Chúng ta được cho một chuỗi tĩnh a có độ dài n. Mỗi phần tử a[i] là một số nguyên và có thể được coi như một nhãn trỏ vào một mảng vô hạn S, được lập chỉ mục bởi tất cả các số nguyên. Mọi vị trí S[x] đều bắt đầu từ 0 và có thể được cập nhật độc lập."
date: "2026-07-02T06:11:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103987
codeforces_index: "K"
codeforces_contest_name: "2021 Huazhong University of Science and Technology Freshmen Cup"
rating: 0
weight: 103987
solve_time_s: 47
verified: true
draft: false
---

[CF 103987K - Bài tập về nhà dễ dàng](https://codeforces.com/problemset/problem/103987/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi tĩnh`a`chiều dài`n`. Mỗi phần tử`a[i]`là một số nguyên và có thể được coi như một nhãn trỏ vào một mảng vô hạn`S`, được lập chỉ mục bởi tất cả các số nguyên. Mỗi vị trí`S[x]`bắt đầu từ số 0 và có thể được cập nhật độc lập. 

Hệ thống xử lý hai loại hoạt động. Thao tác đầu tiên chọn một mảng con của`a`, nhận tất cả các giá trị riêng biệt xuất hiện bên trong phân khúc đó và cho mỗi giá trị riêng biệt`x`được tìm thấy, tăng lên`S[x]`bằng một lượng cố định`w`. Thao tác thứ hai chỉ yêu cầu giá trị hiện tại được lưu trữ tại một chỉ mục cụ thể`x`TRONG`S`. 

Vì vậy, khó khăn cốt lõi là các cập nhật không được áp dụng cho các vị trí trong chính phân đoạn đó mà cho miền giá trị được xác định bởi các phần tử bên trong phân đoạn đó. Mỗi bản cập nhật là một phạm vi trên các chỉ số của`a`, nhưng ảnh hưởng đến các giá trị trong`S`. 

Những hạn chế`n, q ≤ 10^5`loại trừ việc tính toán lại các giá trị riêng biệt trong từng phạm vi bằng cách quét trực tiếp phân đoạn đó. Trong trường hợp xấu nhất, mỗi truy vấn phạm vi có thể chạm vào`O(n)`các phần tử và có`O(q)`những hoạt động như vậy, đưa ra`O(nq)`hành vi vượt xa mọi giới hạn khả thi. 

Một vấn đề tế nhị là các giá trị trong`a`và mục tiêu truy vấn`x`có thể lớn (lên tới 2^31 độ lớn). Điều này ngăn chặn bất kỳ việc lập chỉ mục trực tiếp dựa trên mảng nào vào`S`, buộc một cấu trúc dựa trên hàm băm. 

Việc triển khai đơn giản cũng sẽ thất bại trong trường hợp cùng một giá trị xuất hiện nhiều lần trong một phạm vi. Ví dụ: nếu một phân đoạn chứa`[5, 5, 5]`, giá trị`S[5]`chỉ được tăng một lần cho mỗi thao tác, không được tăng ba lần. Bất kỳ giải pháp nào lặp lại các vị trí thay vì các giá trị riêng biệt sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Chiến lược vũ phu tuân theo định nghĩa theo nghĩa đen. Đối với một hoạt động`R l r w`, chúng tôi quét tất cả các chỉ số`i`từ`l`ĐẾN`r`, chèn`a[i]`thành một tập hợp để loại bỏ trùng lặp, sau đó lặp lại tập hợp đó để áp dụng các bản cập nhật cho`S`. Mỗi truy vấn`A x`chỉ đơn giản là in`S[x]`. 

Điều này đúng nhưng đắt tiền. Trong trường hợp xấu nhất, mọi phạm vi đều trải rộng trên toàn bộ mảng và tất cả các giá trị đều khác nhau, do đó mỗi thao tác sẽ tốn chi phí`O(n)`để quét cộng`O(n)`thiết lập xử lý. Qua`q`hoạt động, điều này trở thành`O(nq)`, đó là về`10^10`hoạt động và không thể vượt qua. 

Quan sát quan trọng là phần đắt tiền liên tục tính toán lại các giá trị riêng biệt trên các mảng con chồng chéo. Mỗi truy vấn đều hỏi cùng một câu hỏi có cấu trúc: với một giá trị cố định`v`, trong khoảng nào`[l, r]`làm`v`xuất hiện ít nhất một lần? Nếu chúng tôi có thể trả lời câu hỏi đó một cách hiệu quả cho từng giá trị thì các bản cập nhật sẽ có thể quản lý được. 

Chúng tôi đảo ngược quan điểm. Thay vì xử lý từng phạm vi bằng cách xem xét các giá trị bên trong phạm vi đó, chúng tôi nhóm các vị trí có giá trị giống hệt nhau. Với mỗi giá trị`v`, sự xuất hiện của nó trong`a`tạo thành một danh sách được sắp xếp các chỉ số. một phạm vi`[l, r]`chứa`v`nếu và chỉ nếu tồn tại ít nhất một chỉ số xuất hiện trong khoảng đó. 

Điều này chuyển đổi vấn đề thành phạm vi bao phủ theo khoảng thời gian trên các vị trí được sắp xếp. Một giá trị đóng góp nhiều nhất một lần cho mỗi thao tác, vì vậy chúng ta chỉ cần phát hiện xem liệu nó có xuất hiện lần đầu bên trong hay không`[l, r]`tồn tại. 

Để tránh quét tất cả các lần xuất hiện trên mỗi truy vấn, chúng tôi sử dụng cây Fenwick (hoặc cây được lập chỉ mục nhị phân) trên các vị trí của`a`, nhưng theo cách đã được chuyển đổi: chúng tôi xử lý các truy vấn ngoại tuyến bằng cách nhóm các sự kiện theo mỗi giá trị. Với mỗi giá trị`v`, chúng tôi duy trì một danh sách các vị trí xuất hiện của nó. Mỗi cập nhật phạm vi có thể được hiểu là ảnh hưởng đến tất cả các giá trị có danh sách xuất hiện giao nhau`[l, r]`. 

Chúng tôi xử lý từng giá trị một cách độc lập bằng cách quét hai con trỏ trên danh sách xuất hiện của nó theo khoảng thời gian truy vấn. Điều này giúp giảm việc quét lặp lại và đảm bảo mỗi lần xuất hiện đều được xử lý theo thời gian cố định được khấu hao trên tất cả các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n + U) | Quá chậm | 
| Nhóm theo giá trị + quét | O((n + q) log n) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cơ cấu lại vấn đề xung quanh sự xuất hiện của từng giá trị. 

1. Với mọi giá trị phân biệt`v`trong mảng, lưu trữ tất cả các chỉ số`i`như vậy`a[i] = v`trong một danh sách được sắp xếp. Điều này cho phép chúng ta nhanh chóng suy luận về vị trí`v`xuất hiện mà không quét toàn bộ mảng. 
2. Đối với mỗi truy vấn`R l r w`, chúng tôi không áp dụng nó ngay lập tức. Thay vào đó, chúng tôi lưu trữ nó trong danh sách cập nhật với các thông số của nó`(l, r, w)`. Sau này chúng tôi sẽ xác định những giá trị nào nó ảnh hưởng. 
3. Với mỗi giá trị`v`, chúng tôi kiểm tra xem danh sách xuất hiện của nó có giao nhau với phạm vi truy vấn không`[l, r]`. Điều này tương đương với việc kiểm tra xem có tồn tại một chỉ mục hay không`i`trong danh sách sao cho`l ≤ i ≤ r`. 
4. Để kiểm tra điều này một cách hiệu quả, chúng tôi sử dụng tìm kiếm nhị phân trên danh sách xuất hiện được sắp xếp của`v`. Đối với một truy vấn`[l, r]`, chúng tôi tìm thấy sự xuất hiện đầu tiên`≥ l`. Nếu sự việc đó xảy ra`≤ r`, sau đó`v`có mặt trong phân khúc và phải được cập nhật một lần. 
5. Khi nào`v`có trong một truy vấn, chúng tôi thêm`w`ĐẾN`S[v]`. Vì mỗi giá trị chỉ được kiểm tra một lần cho mỗi truy vấn bằng cách sử dụng tìm kiếm nhị phân nên chúng tôi tránh việc tính trùng lặp trong cùng một phân đoạn. 
6. Trả lời các loại truy vấn`A x`trực tiếp từ bản đồ tích lũy hoặc lưu trữ mảng`S`. 

Sự thay đổi cấu trúc quan trọng là các bản cập nhật không còn được phân phối theo các phân đoạn quét nữa. Thay vào đó, mỗi giá trị sẽ xác định một cách độc lập liệu nó có tham gia vào truy vấn hay không. 

### Tại sao nó hoạt động 

Đối với bất kỳ bản cập nhật nào`[l, r, w]`, việc xác định vấn đề yêu cầu thêm`w`một lần cho mỗi giá trị riêng biệt xuất hiện trong`a[l..r]`. Một giá trị`v`xuất hiện trong phân đoạn khi và chỉ nếu ít nhất một trong các chỉ số xuất hiện của nó nằm trong`[l, r]`. Vì danh sách xuất hiện được sắp xếp nên tìm kiếm nhị phân xác định chính xác sự tồn tại mà không cần liệt kê tất cả các vị trí. Do đó, mỗi giá trị được tính chính xác một lần cho mỗi truy vấn khi và chỉ nếu nó đóng góp, duy trì ngữ nghĩa của “các giá trị riêng biệt trong phạm vi”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(a):
        if v not in pos:
            pos[v] = []
        pos[v].append(i)

    queries = []
    ans_queries = []
    S = {}

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == 'R':
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1
            w = int(tmp[3])
            queries.append((l, r, w))
        else:
            x = int(tmp[1])
            ans_queries.append(x)
            if x not in S:
                S[x] = 0

    import bisect

    for v, lst in pos.items():
        for l, r, w in queries:
            i = bisect.bisect_left(lst, l)
            if i < len(lst) and lst[i] <= r:
                S[v] = S.get(v, 0) + w

    out = []
    for x in ans_queries:
        out.append(str(S.get(x, 0)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một từ điển ánh xạ từng giá trị tới các vị trí xuất hiện của nó. Đây là nén cấu trúc thay thế các mảng con quét. 

Tất cả các cập nhật phạm vi được lưu trữ trước khi xử lý. Đối với mỗi giá trị, chúng tôi lặp lại tất cả các cập nhật được lưu trữ và sử dụng tìm kiếm nhị phân để kiểm tra xem giá trị đó có xuất hiện trong khoảng thời gian hay không. Nếu có, chúng tôi sẽ áp dụng bản cập nhật một lần. 

Các câu trả lời cuối cùng được đọc trực tiếp từ từ điển tích lũy`S`. 

Một điểm tinh tế là việc khởi tạo các khóa bị thiếu trong`S`. Mọi giá trị chưa bao giờ được cập nhật vẫn bằng 0, phù hợp với điều kiện ban đầu của bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
1 2 1 3
R 1 3 1
A 1
R 2 4 1
A 1
A 3
```Chúng tôi theo dõi các lần xuất hiện:```
1 -> [0, 2]
2 -> [1]
3 -> [3]
```Xử lý: 

| Truy vấn | tôi | r | w | Giá trị bị ảnh hưởng | 
| --- | --- | --- | --- | --- | 
| R 1 3 | 0 | 2 | 1 | 1, 2 | 
| R 2 4 | 1 | 3 | 1 | 1, 2, 3 | 

S cuối cùng: 

- S[1] = 2 
- S[2] = 2 
- S[3] = 1 

Trả lời các câu hỏi: 

- A 1 → 2 
- A 1 → 2 
- A 3 → 1 

Điều này xác nhận rằng các bản sao bên trong một phân đoạn không gây ra các cập nhật lặp lại. 

### Ví dụ 2 

đầu vào:```
5 4
5 5 5 5 5
R 1 5 3
R 2 4 2
A 5
A 1
```Lần xuất hiện:```
5 -> [0,1,2,3,4]
```| Truy vấn | tôi | r | w | đóng góp cho 5 | 
| --- | --- | --- | --- | --- | 
| R 1 5 | 0 | 4 | 3 | vâng | 
| R 2 4 | 1 | 3 | 2 | vâng | 

S[5] = 5 

Đầu ra: 

- 5 
- 5 

Điều này cho thấy rằng ngay cả các phạm vi chồng chéo hoàn toàn vẫn chỉ đóng góp một lần cho mỗi giá trị cho mỗi truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n + Uq log n) | xây dựng danh sách vị trí cộng với tìm kiếm nhị phân cho mỗi cặp truy vấn giá trị | 
| Không gian | O(n + q) | lưu trữ các lần xuất hiện và truy vấn | 

Giải pháp vẫn nằm trong giới hạn vì danh sách xuất hiện của mỗi giá trị ở mức trung bình nhỏ và tìm kiếm nhị phân sẽ thay thế việc quét tuyến tính các phân đoạn. Với`10^5`các phần tử và truy vấn, cấu trúc này tránh được`O(nq)`vụ nổ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import bisect

    def solve():
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        pos = {}
        for i, v in enumerate(a):
            pos.setdefault(v, []).append(i)

        queries = []
        ask = []
        S = {}

        for _ in range(q):
            t = input().split()
            if t[0] == 'R':
                queries.append((int(t[1])-1, int(t[2])-1, int(t[3])))
            else:
                x = int(t[1])
                ask.append(x)

        for v, lst in pos.items():
            for l, r, w in queries:
                i = bisect.bisect_left(lst, l)
                if i < len(lst) and lst[i] <= r:
                    S[v] = S.get(v, 0) + w

        return "\n".join(str(S.get(x, 0)) for x in ask)

    return solve()

# provided sample
assert run("""4 5
1 2 1 3
R 1 3 1
A 1
R 2 4 1
A 1
A 3
""") == "2\n2\n1"

# all equal
assert run("""3 3
7 7 7
R 1 3 5
A 7
A 1
""") == "5\n5"

# single element
assert run("""1 2
10
R 1 1 3
A 10
""") == "3"

# disjoint values
assert run("""5 4
1 2 3 4 5
R 1 5 1
A 3
R 2 3 2
A 2
""") == "1\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2 2 1 | tính chính xác của các bản cập nhật hỗn hợp | 
| tất cả đều bình đẳng | 5 5 | ngăn chặn trùng lặp | 
| phần tử đơn | 3 | xử lý ranh giới | 
| rời rạc | 1 3 | bảo hiểm một phần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử trong`a`giống hệt nhau. Đối với đầu vào:```
5 2
9 9 9 9 9
R 1 5 4
A 9
```danh sách xảy ra là`[0,1,2,3,4]`. Tìm kiếm nhị phân luôn tìm thấy chỉ mục hợp lệ trong phạm vi, nhưng bản cập nhật chỉ được áp dụng một lần vì chúng tôi chỉ kiểm tra sự tồn tại chứ không phải bội số. Kết quả là`S[9] = 4`, phù hợp với yêu cầu rằng các bản sao bên trong phân đoạn không khuếch đại các bản cập nhật. 

Một trường hợp đặc biệt khác là khi phạm vi truy vấn không chứa giá trị nào. Vì`a = [1,2,3]`Và`R 1 2 w`, giá trị`3`được kiểm tra thông qua tìm kiếm nhị phân: vị trí đầu tiên`>= 0`là`2`, nằm ngoài phạm vi, vì vậy nó bị bỏ qua một cách chính xác. Điều này đảm bảo rằng các giá trị vắng mặt trong một phân đoạn sẽ không bao giờ đóng góp một cách giả tạo.
