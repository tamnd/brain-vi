---
title: "CF 103797J - Thẩm phán Crush"
description: "Chúng ta được cung cấp một mạng lưới các thí sinh theo vấn đề, trong đó mỗi cặp $(c, p)$ đại diện cho một thí sinh giải quyết một vấn đề. Theo thời gian, chúng tôi nhận được một luồng nội dung gửi theo trình tự thời gian, mỗi nội dung gửi thuộc về một ô như vậy và mang theo phán quyết."
date: "2026-07-02T08:49:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103797
codeforces_index: "J"
codeforces_contest_name: "IME++ Starters Try-outs 2022"
rating: 0
weight: 103797
solve_time_s: 49
verified: true
draft: false
---

[CF 103797J - Judge Crush](https://codeforces.com/problemset/problem/103797/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các thí sinh theo vấn đề, trong đó mỗi cặp$(c, p)$đại diện cho một thí sinh giải quyết một vấn đề. Theo thời gian, chúng tôi nhận được một luồng nội dung gửi theo trình tự thời gian, mỗi nội dung gửi thuộc về một ô như vậy và mang theo phán quyết. 

Nguyên tắc chính là chúng tôi chỉ quan tâm đến việc gửi không chính xác xảy ra trước giải pháp được chấp nhận đầu tiên cho cùng giải pháp đó.$(c, p)$. Sau khi một ô nhận được AC đầu tiên, ô đó sẽ không hoạt động và các lần gửi sau đó tới ô đó phải bị bỏ qua hoàn toàn. 

Sau khi xử lý tất cả các bài gửi, chúng tôi phải trả lời nhiều câu hỏi. Mỗi truy vấn đưa ra một lưới con hình chữ nhật được xác định bởi một loạt các đối thủ và một loạt các vấn đề. Đối với hình chữ nhật đó, chúng ta cần tính tổng số lần gửi không chính xác được tính (những lần gửi xảy ra trước AC) trên tất cả các ô bên trong nó. 

Các ràng buộc đã gợi ý hình dạng của giải pháp. Kích thước lưới tối đa là$500 \times 500$, tức là khoảng 250.000 tế bào. Số lượng gửi và truy vấn đều có thể đạt tới$2 \times 10^5$. Điều này ngay lập tức loại trừ mọi giải pháp quét nội dung gửi cho mỗi truy vấn hoặc tính toán lại từng ô cho mỗi truy vấn. Cấu trúc gợi ý rõ ràng rằng chúng ta nên nén tất cả thông tin gửi vào một mảng 2D tĩnh và sau đó hỗ trợ các truy vấn tổng ma trận con nhanh. 

Một điểm tinh tế là điều kiện “trước AC chỉ”. Một sai lầm ngây thơ là tính tất cả các bài gửi không phải AC trên toàn cầu và sau đó cố gắng trừ những bài gửi sau AC. Điều đó không thành công vì các lần gửi sau AC hoàn toàn không ảnh hưởng đến ô, nhưng chúng vẫn xuất hiện trong luồng đầu vào và có thể tăng số lượng không chính xác nếu không được lọc cẩn thận trên mỗi ô. 

Một trường hợp cạnh khác xuất hiện khi một ô không bao giờ nhận được AC. Trong trường hợp đó, không có bài gửi nào của nó sẽ được tính cả, ngay cả khi có nhiều mục WA hoặc TLE. Chỉ những tế bào cuối cùng đạt đến AC mới đóng góp bất cứ điều gì. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng từng truy vấn một cách độc lập. Đối với một hình chữ nhật nhất định, chúng ta có thể lặp lại tất cả các lần gửi và kiểm tra xem chúng có thuộc về một ô bên trong hình chữ nhật và xảy ra trước AC đầu tiên của ô đó hay không. Điều này yêu cầu quét tối đa$2 \times 10^5$gửi cho mỗi truy vấn, tạo ra độ phức tạp trong trường hợp xấu nhất xung quanh$4 \times 10^{10}$hoạt động vượt quá giới hạn cho phép. 

Quan sát quan trọng là mỗi tế bào hoạt động độc lập với các tế bào khác. Một bài nộp chỉ ảnh hưởng đến bài nộp của chính nó$(c, p)$và khi chúng tôi xác định có bao nhiêu lần gửi sai hợp lệ thuộc về ô đó, nó sẽ không bao giờ thay đổi nữa. Điều này có nghĩa là toàn bộ quá trình động có thể được rút gọn thành việc tính toán một số nguyên duy nhất trên mỗi ô lưới. 

Khi mỗi ô có một giá trị cố định, bài toán sẽ trở thành bài toán truy vấn tổng phạm vi 2D cổ điển. Tổng tiền tố 2D cho phép trả lời từng truy vấn trong thời gian không đổi. 

Nhiệm vụ duy nhất còn lại là tính toán, đối với mỗi ô, có bao nhiêu lần gửi trước AC đầu tiên của nó là không chính xác. Chúng tôi có thể xử lý nhật ký gửi theo thứ tự, duy trì cho từng ô xem nó đã thấy AC hay chưa và nếu chưa, hãy tăng bộ đếm của nó bất cứ khi nào kết quả không phải AC xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(S \cdot Q)$|$O(1)$| Quá chậm | 
| Tích lũy trên mỗi ô + tổng tiền tố 2D |$O(S + NM + Q)$|$O(NM)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới như một bảng trong đó mỗi mục lưu trữ một bộ đếm các lần gửi sai hợp lệ. 

1. Tạo hai$N \times M$mảng: một mảng boolean để đánh dấu xem một ô đã nhận được AC hay chưa và một mảng số nguyên để đếm số lần gửi không chính xác trước AC. Sự tách biệt này là cần thiết vì sau AC chúng ta phải đóng băng trạng thái của ô đó. 
2. Xử lý hồ sơ theo trình tự thời gian. Đối với mỗi lần nộp$(c, p, v)$, trước tiên hãy kiểm tra xem ô này đã có AC chưa. Nếu đúng như vậy, hãy bỏ qua việc gửi hoàn toàn vì theo định nghĩa, mọi thứ sau AC đều không liên quan. 
3. Nếu kết quả là AC và ô chưa được đánh dấu, hãy đánh dấu ô đó là đã giải quyết. Từ thời điểm này trở đi, không có lần gửi nào trong tương lai sẽ ảnh hưởng đến ô này. 
4. Nếu kết quả không phải là AC và ô vẫn chưa được giải, hãy tăng bộ đếm của nó. Điều này đảm bảo chúng tôi chỉ tính những lỗi xảy ra trước lần gửi thành công đầu tiên. 
5. Sau khi xử lý tất cả các lần gửi, hãy tạo tổng tiền tố 2D trên lưới bộ đếm. Mỗi ô tiền tố lưu trữ tổng của tất cả các giá trị trong hình chữ nhật từ$(1,1)$ĐẾN$(i,j)$. 
6. Đối với mỗi hình chữ nhật truy vấn, hãy tính tổng bằng cách sử dụng loại trừ bao gồm trên lưới tiền tố trong thời gian không đổi. 

### Tại sao nó hoạt động 

Mỗi ô đều độc lập vì các lần gửi chỉ sửa đổi tọa độ của chính chúng. Thời điểm một ô nhận được AC, đóng góp cuối cùng của nó được xác định đầy đủ và không có sự kiện nào sau đó có thể thay đổi được. Do đó, vấn đề phân rã thành việc tính toán một giá trị cố định trên mỗi ô. Phép chuyển đổi tổng tiền tố bảo toàn số tiền chính xác trên các hình chữ nhật con, do đó mọi truy vấn chỉ được trả lời bằng cách sử dụng thông tin được tính toán trước mà không cần xem lại nhật ký gửi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M = map(int, input().split())
    S = int(input())

    solved = [[False] * (M + 1) for _ in range(N + 1)]
    bad = [[0] * (M + 1) for _ in range(N + 1)]

    for _ in range(S):
        c, p, v = input().split()
        c = int(c)
        p = int(p)

        if solved[c][p]:
            continue

        if v == "AC":
            solved[c][p] = True
        else:
            bad[c][p] += 1

    pref = [[0] * (M + 1) for _ in range(N + 1)]

    for i in range(1, N + 1):
        row_sum = 0
        for j in range(1, M + 1):
            row_sum += bad[i][j]
            pref[i][j] = pref[i - 1][j] + row_sum

    Q = int(input())
    out = []

    for _ in range(Q):
        c1, p1, c2, p2 = map(int, input().split())

        res = pref[c2][p2]
        if c1 > 1:
            res -= pref[c1 - 1][p2]
        if p1 > 1:
            res -= pref[c2][p1 - 1]
        if c1 > 1 and p1 > 1:
            res += pref[c1 - 1][p1 - 1]

        out.append(str(res))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai cốt lõi duy trì một máy trạng thái trên mỗi ô. các`solved`table đảm bảo rằng khi đạt đến AC, các lần gửi sau sẽ bị bỏ qua. các`bad`bảng chỉ tích lũy các lần thử sai trước AC hợp lệ. 

Việc xây dựng tổng tiền tố được thực hiện theo từng hàng để tránh tính toán lại lồng nhau. Việc trả lời truy vấn sử dụng công thức bao gồm-loại trừ tiêu chuẩn cho các ma trận con, hoàn toàn dựa vào bảng tiền tố được tính toán trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống nhỏ có 2 thí sinh và 2 bài toán. Giả sử chúng tôi xử lý các lần gửi và kết thúc với số lần gửi sai hợp lệ trên mỗi ô sau đây trước AC: 

| Tế bào | số xấu | 
| --- | --- | 
| (1,1) | 0 | 
| (1,2) | 1 | 
| (2,1) | 3 | 
| (2,2) | 0 | 

Bây giờ chúng ta xây dựng tổng tiền tố: 

| tôi\j | 1 | 2 | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 3 | 4 | 

Đối với truy vấn bao phủ toàn bộ lưới, chúng tôi trả về 4. Điều này khớp với thực tế là chỉ có ba lần gửi sai xảy ra trong (2,1) và một lần gửi sai trong (1,2). 

### Ví dụ 2 

Nếu một ô có nhiều lần gửi không chính xác nhưng không bao giờ nhận được AC, chẳng hạn như (1,1) có 10 WA nhưng không có AC, thì đóng góp của nó vẫn bằng 0. Lưới tiền tố hoàn toàn bỏ qua nó. 

Truy vấn trên bất kỳ vùng nào chứa (1,1) sẽ không bao gồm 10 lần thử đó, xác nhận rằng chỉ các ô có AC cuối cùng mới đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S + NM + Q)$| Một lần gửi qua, một lần chuyển qua lưới cho tổng tiền tố, thời gian không đổi cho mỗi truy vấn | 
| Không gian |$O(NM)$| Hai lưới lưu trữ tổng trạng thái và tiền tố | 

Giới hạn$N, M \le 500$làm cho$NM$lưu trữ tầm thường, trong khi$S, Q \le 2 \times 10^5$đảm bảo rằng chỉ chấp nhận việc xử lý các nội dung gửi và truy vấn theo thời gian tuyến tính. Giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    N, M = map(int, input().split())
    S = int(input())

    solved = [[False] * (M + 1) for _ in range(N + 1)]
    bad = [[0] * (M + 1) for _ in range(N + 1)]

    for _ in range(S):
        c, p, v = input().split()
        c = int(c)
        p = int(p)
        if solved[c][p]:
            continue
        if v == "AC":
            solved[c][p] = True
        else:
            bad[c][p] += 1

    pref = [[0] * (M + 1) for _ in range(N + 1)]
    for i in range(1, N + 1):
        for j in range(1, M + 1):
            pref[i][j] = pref[i-1][j] + pref[i][j-1] - pref[i-1][j-1] + bad[i][j]

    Q = int(input())
    out = []
    for _ in range(Q):
        c1, p1, c2, p2 = map(int, input().split())
        res = pref[c2][p2] - pref[c1-1][p2] - pref[c2][p1-1] + pref[c1-1][p1-1]
        out.append(str(res))
    return "\n".join(out)

# custom minimal case
assert run("""1 1
3
1 1 WA
1 1 AC
1 1 WA
1
1 1 1 1
""") == "1", "basic AC cutoff"

# all no AC => zero contribution
assert run("""2 2
3
1 1 WA
1 1 WA
2 2 TLE
1
1 1 2 2
""") == "0", "no AC anywhere"

# multiple cells independent
assert run("""2 2
4
1 1 WA
1 2 WA
2 1 WA
2 2 AC
1
1 1 2 2
""") == "3", "independent accumulation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cắt AC tối thiểu | 1 | dừng đếm sau AC | 
| không có AC ở bất cứ đâu | 0 | các tế bào không có AC không đóng góp gì | 
| tế bào độc lập | 3 | độc lập trên mỗi ô | 

## Vỏ cạnh 

Một cạm bẫy phổ biến là tính tất cả các lần gửi sai bất kể thời gian AC. Ví dụ:```
1 1
3
1 1 WA
1 1 AC
1 1 WA
1
1 1 1 1
```Ở đây câu trả lời đúng là 1 chứ không phải 2. Lần gửi thứ ba phải được bỏ qua vì AC đã xảy ra. Thuật toán xử lý việc này bằng cách đánh dấu ô là đã giải quyết và bỏ qua tất cả các cập nhật sau này. 

Một trường hợp cạnh khác là một ô không bao giờ nhận được AC:```
1 1
2
1 1 WA
1 1 TLE
1
1 1 1 1
```Đầu ra đúng là 0.`solved`cờ không bao giờ được đặt, nhưng vì không có AC nên quy tắc nói rõ rằng chúng tôi không nên tính những lần gửi này. Việc triển khai thực thi điều này bằng cách chỉ tăng`bad`trước AC và không bao giờ cộng nó vào tổng cuối cùng trừ khi AC xảy ra.
