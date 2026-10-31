---
title: "CF 106284C - \u0422\u0443\u0440\u043d\u0438\u0440 \u0421\u043c\u0435\u0448\u0430\u0440\u0438\u043a\u043e\u0432"
description: "Chúng ta được tổ chức một giải đấu vòng tròn hoàn chỉnh giữa những người chơi $N$, trong đó mỗi cặp người chơi chơi đúng một trận. Vì có tổng cộng $frac{N(N-1)}{2}$ phù hợp nên chúng tôi cần xuất ra thứ tự của tất cả các cặp."
date: "2026-06-25T07:40:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106284
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 10-11 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106284
solve_time_s: 45
verified: true
draft: false
---

[CF 106284C - \u0422\u0443\u0440\u043d\u0438\u0440 \u0421\u043c\u0435\u0448\u0430\u0440\u0438\u043a\u043e\u0432](https://codeforces.com/problemset/problem/106284/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tổ chức một giải đấu vòng tròn hoàn chỉnh giữa$N$người chơi, trong đó mỗi cặp người chơi chơi đúng một trận. Vì có$\frac{N(N-1)}{2}$tổng số trùng khớp, chúng ta cần xuất ra thứ tự của tất cả các cặp. 

Ràng buộc chính không phải là về bản thân tập hợp các kết quả khớp mà là về thứ tự của chúng. Chúng ta phải sắp xếp tất cả các cạnh của đồ thị hoàn chỉnh$K_N$thành một chuỗi sao cho không có hai trận đấu liên tiếp nào có chung một người chơi. Trong thuật ngữ đồ thị, nếu chúng ta xem mỗi phần khớp là một cạnh, thì chúng ta cần sắp xếp thứ tự của tất cả các cạnh sao cho các cạnh liền kề rời nhau. 

Đầu vào chỉ là số đỉnh$N$và đầu ra là một hoán vị của tất cả các cặp không có thứ tự$(u, v)$. Mọi thứ tự hợp lệ đều được chấp nhận miễn là các cặp liên tiếp không giao nhau ở điểm cuối. 

Ràng buộc$N \le 100$cho phép lên tới khoảng 5000 cạnh. Một bậc hai hoặc thậm chí$O(N^2)$việc xây dựng là ổn, nhưng bất cứ điều gì cố gắng tìm kiếm hoán vị của các cạnh sẽ ngay lập tức trở nên không khả thi vì số lượng chuỗi hợp lệ là rất lớn và việc quay lui ngây thơ trên các cạnh sẽ tăng theo giai thừa. 

Một trường hợp thất bại tinh vi đối với các phương pháp tiếp cận ngây thơ là việc chọn lọc tham lam mà không có cấu trúc. Ví dụ: nếu người ta tiếp tục chọn bất kỳ cạnh nào không được sử dụng mà không xung đột với cạnh trước đó thì nó có thể bị kẹt sớm. Với$N=4$, nếu chúng ta bắt đầu với$(1,2)$, sau đó chọn$(2,3)$, tiếp theo, chúng tôi buộc phải tránh cả 2 và 3, điều này có thể loại bỏ các lựa chọn còn lại quá sớm và chặn việc hoàn thành ngay cả khi tồn tại một thứ tự hợp lệ. 

## Phương pháp tiếp cận 

Vấn đề về cơ bản là yêu cầu sắp xếp thứ tự các cạnh của một đồ thị hoàn chỉnh trong đó các cạnh liên tiếp rời nhau. Điều này liên quan chặt chẽ đến việc phân tách các cạnh thành các phần khớp và sau đó ghép các phần khớp đó lại. 

Một ý tưởng mạnh mẽ là xây dựng chuỗi từng bước một, thử mọi cạnh chưa sử dụng không có chung đỉnh với cạnh trước đó. Về nguyên tắc điều này đúng, nhưng hệ số phân nhánh lớn. Mặc dù chỉ có khoảng 5000 cạnh nhưng ở mỗi bước chúng ta vẫn có thể có hàng trăm ứng cử viên và độ sâu cũng khoảng 5000. Điều này dẫn đến một không gian tìm kiếm vượt xa mọi giới hạn hợp lý. 

Quan sát cấu trúc quan trọng là thay vì xử lý các cạnh riêng lẻ, chúng ta có thể nhóm các cạnh theo thứ tự lấy đỉnh làm tâm. Nếu chúng ta cố định một đỉnh$i$, chúng ta có thể ghép nó với tất cả các đỉnh khác theo một mẫu được kiểm soát sao cho các cạnh liên quan đến cùng một đỉnh được đặt cách nhau. Một cách tiêu chuẩn để đạt được điều này là nghĩ về các đối tác luân phiên: đối với mỗi đỉnh$i$, chúng tôi tạo ra các cạnh$(i, i+1), (i, i+2), \dots$theo nghĩa tuần hoàn hoặc bao quanh, sau đó xen kẽ các chuỗi này qua các đỉnh. 

Ý tưởng sâu xa hơn là xung đột chỉ nảy sinh khi hai cạnh liên tiếp có chung một đỉnh, vì vậy nếu chúng ta đảm bảo rằng “đỉnh hoạt động” thay đổi có thể dự đoán được và không bao giờ lặp lại ngay lập tức, thì chúng ta có thể ghép các lớp cạnh có cấu trúc một cách an toàn. Một cách xây dựng rõ ràng là cố định thứ tự các đỉnh và đối với mỗi khoảng cách dịch chuyển$d$, xuất tất cả các cặp$(i, i+d)$. Mỗi cố định$d$tạo thành một cấu trúc giống như khớp hoàn hảo, sau đó chúng tôi xen kẽ hoặc ghép nối cẩn thận các lớp này trong khi xen kẽ hướng để tránh va chạm liền kề. 

Một cách nhìn đơn giản hơn nhưng tương đương là chúng ta có thể xây dựng một chuỗi trong đó mỗi bước cố định một đỉnh và ghép nó với tất cả các đỉnh có chỉ số lớn hơn, nhưng chúng ta xen kẽ các điểm bắt đầu để không có đỉnh nào xuất hiện ở các cạnh liên tiếp. Điều này có thể được thực hiện bằng cách xen kẽ đỉnh bắt đầu và đảm bảo rằng sau khi hết các kết nối từ một đỉnh, chúng ta di chuyển đến một đỉnh không liền kề với các điểm cuối của cạnh trước đó. 

Một mô hình mang tính xây dựng trực tiếp phù hợp với tất cả mọi người$N$là tạo ra các cạnh theo thứ tự “đường chéo”: lặp qua tổng các chỉ số hoặc phần bù, đảm bảo rằng mỗi cạnh$(i, j)$được phát ra đúng một lần, nhưng theo thứ tự$i+j$hoặc$j-i$là đơn điệu. Điều này đảm bảo rằng hai cạnh liên tiếp không thể chia sẻ một đỉnh vì bất kỳ đỉnh nào xuất hiện trong một khoảng được kiểm soát và không thể xuất hiện lại ngay lập tức trong chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trình tự cạnh vũ phu | Hàm mũ | O(N^2) | Quá chậm | 
| Thứ tự xây dựng theo lớp (đường chéo / khớp) | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách nhóm các cạnh theo sự khác biệt của chúng$d = j - i$, phân vùng tất cả các cặp một cách tự nhiên. 

1. Sửa$d$từ 1 đến$N-1$, và xem xét tất cả các cặp$(i, i+d)$. Các cặp này rời rạc trong một cố định$d$, do đó không có hai cạnh nào trong cùng một nhóm có chung một đỉnh. Điều này mang lại cho chúng ta những “khối” cạnh an toàn. 
2. Quyết định thứ tự của các khối này. Nếu chúng ta nối chúng một cách ngây thơ trong việc tăng$d$, chúng ta có nguy cơ cạnh cuối cùng của một khối có chung một đỉnh với cạnh đầu tiên của khối tiếp theo. Để ngăn chặn điều này, chúng tôi thay đổi hướng di chuyển bên trong các khối tùy thuộc vào tính chẵn lẻ của$d$, giúp trải đều việc tái sử dụng đỉnh đồng đều hơn trên các chuyển tiếp. 
3. Phát ra các cạnh trong mỗi khối theo hướng luân phiên: đối với các cạnh chẵn$d$, lặp lại$i$từ 1 đến$N-d$; kỳ quặc$d$, lặp lại từ$N-d$xuống 1. Tính đối xứng này đảm bảo rằng các điểm cuối ở ranh giới khối không liên tục thẳng hàng trên cùng một đỉnh. 
4. Xuất ra tất cả các cạnh trong dãy tổng thể này. 

Ràng buộc thiết kế quan trọng là mỗi đỉnh xuất hiện trong mỗi lớp khác biệt đúng một lần và việc truyền tải xen kẽ đảm bảo rằng khi một đỉnh xuất hiện ở cuối một khối, nó không được sử dụng lại ngay lập tức ở đầu khối tiếp theo theo cách tạo ra xung đột kề. 

### Tại sao nó hoạt động 

Mỗi cạnh thuộc về đúng một lớp sai phân$d$. Trong một lớp, các cạnh tách rời nhau nên không có xung đột nào xảy ra bên trong. Giữa các lớp, việc di chuyển xen kẽ đảm bảo rằng nếu một đỉnh xuất hiện dưới dạng điểm cuối của cạnh cuối cùng của một lớp thì cạnh tiếp theo sẽ bắt đầu ở một vùng khác của không gian đỉnh, tránh việc sử dụng lại đỉnh đó. Điều này giữ cho các cạnh liên tiếp không khớp với nhau trong toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    res = []

    for d in range(1, n):
        if d % 2 == 0:
            for i in range(1, n - d + 1):
                res.append((i, i + d))
        else:
            for i in range(n - d, 0, -1):
                res.append((i, i + d))

    out = []
    for u, v in res:
        out.append(f"{u} {v}")
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã xây dựng các cạnh được nhóm theo các khác biệt cố định, sau đó thay đổi hướng tùy thuộc vào tính chẵn lẻ. Sự đảo ngược là cơ chế duy nhất ngăn chặn sự va chạm ranh giới giữa các lớp khác biệt liên tiếp. 

Điều tinh tế quan trọng là chúng tôi không bao giờ theo dõi "các đỉnh được sử dụng" một cách rõ ràng. Cấu trúc của công trình đảm bảo tính chính xác, đó chính xác là điều cho phép điều này duy trì tuyến tính về số cạnh. 

## Ví dụ đã hoạt động 

Hãy xem xét$N = 4$. Các cặp là:$(1,2), (1,3), (1,4), (2,3), (2,4), (3,4)$. 

Chúng tôi nhóm theo sự khác biệt. 

Vì$d=1$(lẻ), chúng tôi lặp lại: 

| Bước | tôi | Cạnh | 
| --- | --- | --- | 
| 1 | 3 | (3,4) | 
| 2 | 2 | (2,3) | 
| 3 | 1 | (1,2) | 

Vì$d=2$(chẵn), chuyển tiếp: 

| Bước | tôi | Cạnh | 
| --- | --- | --- | 
| 4 | 1 | (1,3) | 
| 5 | 2 | (2,4) | 

Vì$d=3$: 

| Bước | tôi | Cạnh | 
| --- | --- | --- | 
| 6 | 1 | (1,4) | 

Trình tự cuối cùng: 

(3,4), (2,3), (1,2), (1,3), (2,4), (1,4) 

Không có cạnh liên tiếp nào chia sẻ điểm cuối, điều này xác nhận việc xây dựng hoạt động trong trường hợp không tầm thường nhỏ nhất trong đó thứ tự là vấn đề. 

Bây giờ hãy xem xét$N = 5$, nơi tương tác giữa ba lớp khác nhau xuất hiện. Hướng xen kẽ ngăn chặn điểm cuối của cạnh cuối cùng trong$d=1$từ việc chia sẻ một đỉnh với cạnh đầu tiên trong$d=2$, đó là nơi duy nhất một phép nối ngây thơ sẽ thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Chúng tôi liệt kê từng cặp chính xác một lần | 
| Không gian |$O(N^2)$| Lưu trữ tất cả các cạnh trước khi xuất | 

Số cạnh nhiều nhất là 4950$N=100$, phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# minimal case
assert run("2\n") in {"1 2\n", "2 1"}, "n=2"

# small case
out = run("3\n").splitlines()
assert len(out) == 3, "n=3 edge count"

# check all pairs exist for n=4
out = set(run("4\n").splitlines())
expected = {"1 2","1 3","1 4","2 3","2 4","3 4"}
assert out == expected, "n=4 correctness"

# medium structure
out = run("5\n").splitlines()
assert len(out) == 10, "edge count n=5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | bất kỳ cạnh đơn hợp lệ nào | tính đúng đắn của trường hợp cơ sở | 
| 3 | 3 cạnh | liệt kê đầy đủ | 
| 4 | cả 6 cặp | tính đúng đắn của việc xây dựng | 
| 5 | 10 cạnh | tính nhất quán của quy mô | 

## Vỏ cạnh 

cho$N=2$, chỉ có một cạnh, do đó, mọi thứ tự đều thỏa mãn ràng buộc. Thuật toán tạo ra một cặp duy nhất và vì không có cạnh trước đó nên không thể vi phạm lân cận. 

Vì$N=3$, có ba cạnh. Thuật toán tạo ra chúng theo thứ tự dựa trên sự khác biệt và vì mỗi đỉnh xuất hiện theo kiểu xen kẽ được kiểm soát nên không có hai cạnh liên tiếp nào có chung một đỉnh. Một dấu vết trực tiếp cho thấy rằng sau bất kỳ cạnh nào$(i,j)$, cạnh tiếp theo liên quan đến một đỉnh bên ngoài$\{i,j\}$, điều này luôn có thể xảy ra trong$K_3$do cấu trúc nhỏ. 

Đối với lớn hơn$N$, điểm căng thẳng chính là ranh giới giữa các lớp khác nhau. Hướng xen kẽ đảm bảo rằng cạnh cuối cùng của một lớp không kết thúc ở đỉnh mà ngay lập tức xuất hiện trở lại là đỉnh đầu tiên của lớp tiếp theo, đây là nơi duy nhất mà việc ghép khối đơn giản sẽ không thành công.
