---
title: "CF 105317H - Juan vs Man"
description: "Chúng ta được yêu cầu xây dựng một mảng có độ dài $n$, trong đó mọi phần tử nằm trong khoảng từ $1$ đến $m$, với giới hạn toàn cục đối với các mảng con của nó."
date: "2026-06-23T15:13:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "H"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 51
verified: true
draft: false
---

[CF 105317H - Juan vs. Man](https://codeforces.com/problemset/problem/105317/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một mảng có độ dài$n$, trong đó mọi phần tử nằm giữa$1$Và$m$, với sự hạn chế toàn cục đối với các mảng con của nó. Hạn chế không phải là về thứ tự hay tính khác biệt, mà là về tổng tiền tố: không có phân đoạn liền kề nào của mảng được phép có tổng chia hết cho$m$. 

Tương tự, nếu chúng ta định nghĩa tổng tiền tố$s_i = a_1 + a_2 + \dots + a_i$, thì với hai chỉ số bất kỳ$i \le j$, chúng ta phải tránh$s_j \equiv s_{i-1} \pmod m$. Điều kiện “không có tổng mảng con chia hết cho$m$” hoàn toàn giống với việc nói rằng không có hai tổng tiền tố nào có cùng phần dư modulo$m$, ngoại trừ việc chúng tôi ngầm bao gồm$s_0 = 0$. Vì vậy, ràng buộc trở thành: tất cả tiền tố modulo$m$phải khác biệt. 

Đầu vào đưa ra nhiều trường hợp kiểm thử độc lập, mỗi trường hợp chỉ định một cặp$(n, m)$và đối với mỗi trường hợp, chúng tôi xây dựng một mảng hợp lệ hoặc báo cáo rằng điều đó là không thể. 

Các ràng buộc cho phép lên đến$10^5$trường hợp thử nghiệm với tổng số$n$tổng hợp lại$3 \cdot 10^5$. Điều này buộc mọi giải pháp phải chạy về cơ bản theo thời gian tuyến tính trên tất cả các trường hợp thử nghiệm cộng lại. Bất cứ điều gì cố gắng kiểm tra các mảng con một cách rõ ràng đều không thể thực hiện được ngay lập tức vì một mảng có kích thước duy nhất$n$có$O(n^2)$mảng con. 

Một dạng thất bại tinh vi xuất phát từ việc nhầm lẫn tình trạng với điều gì đó cục bộ. Ví dụ, người ta có thể cố gắng đảm bảo rằng không có phần tử đơn lẻ nào có thể chia hết cho$m$, hoặc không có tổng riêng phần nào chỉ bằng 0 ở cuối. Cả hai đều không đủ vì vi phạm có thể xảy ra giữa hai vị trí nội bộ. 

Ví dụ, nếu$m = 5$, mảng$[2, 3, 5]$về mặt phần tử trông có vẻ vô hại, nhưng mảng con$[2, 3]$tổng cộng$5$, chia hết cho$m$. Một kẻ tham lam ngây thơ bỏ qua các tương tác tiền tố sẽ thất bại trong những trường hợp như vậy. 

Một lỗi phổ biến khác là cho rằng vì các giá trị bị giới hạn bởi$m$, chúng ta luôn có đủ sự linh hoạt để tránh va chạm. Trong thực tế, một khi tiền tố tính tổng modulo$m$lặp lại, việc xây dựng sẽ thất bại vĩnh viễn vì sự lặp lại là không thể đảo ngược. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ cố gắng xây dựng mảng từng bước và tại mỗi vị trí, kiểm tra tất cả các giá trị ứng cử viên từ$1$ĐẾN$m$, kiểm tra xem việc thêm nó có tạo ra mảng con bị cấm hay không. Để kiểm tra tính hợp lệ, chúng tôi cần theo dõi tất cả các tổng tiền tố trước đó theo modulo$m$và đảm bảo không xảy ra sự lặp lại. Điều này có thể được thực hiện với một tập hợp băm gồm các phần tiền tố còn lại. 

Tuy nhiên, khó khăn chính là ở mỗi vị trí chúng ta có thể cố gắng đạt tới$m$giá trị và với mỗi lần thử, chúng tôi cập nhật tiền tố và kiểm tra tư cách thành viên. Ngay cả với băm, điều này trở thành$O(nm)$cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất, vượt xa giới hạn khi$m$có thể lên đến$10^6$. 

Cấu trúc của bài toán trở nên rõ ràng hơn khi xem qua tiền tố modulo$m$. Về cơ bản chúng tôi đang cố gắng xây dựng một chuỗi các$n+1$dư lượng tiền tố$s_0, s_1, \dots, s_n$sao cho tất cả đều có modulo riêng biệt$m$. Nhưng chỉ có$m$tổng số dư lượng có thể có. Vì chúng ta đã có$s_0 = 0$, số dư lượng riêng biệt bổ sung tối đa mà chúng ta có thể có là$m-1$. Điều này ngay lập tức ngụ ý một điều kiện cần thiết:$n \le m-1$. 

Một khi chúng ta nhận ra giới hạn này, việc xây dựng sẽ trở nên tự nhiên. Chúng ta chỉ cần duyệt qua các dư lượng riêng biệt theo modulo$m$, không bao giờ lặp lại một. Cách đơn giản nhất là tiếp tục tăng tổng tiền tố theo một bước cố định, thường là$1$và tránh gói modulo$m$quá sớm. Tuy nhiên, sử dụng kích thước bước$1$trực tiếp dẫn đến việc bao phủ toàn bộ các chất cặn theo thứ tự, mang lại một kết cấu xác định rõ ràng. 

Chúng tôi xác định$a_i = 1$cho tất cả$i$, mang lại tổng tiền tố$1, 2, 3, \dots, n$. Đây đều là những modulo riêng biệt$m$miễn là$n < m$. Nếu như$n \ge m$, thì theo nguyên tắc chuồng bồ câu, hai tổng tiền tố phải bằng nhau theo modulo$m$, vì vậy một mảng hợp lệ không thể tồn tại. 

Do đó, vấn đề được chuyển thành một kiểm tra tính khả thi đơn giản, sau đó là việc xây dựng theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc$n$Và$m$. Trước tiên, chúng tôi xác định liệu một công trình hợp lệ có thể thực hiện được hay không. Vì tiền tố tổng theo modulo$m$tất cả đều phải khác biệt và chỉ có$m$dư lượng, chúng tôi yêu cầu$n < m$. Nếu điều kiện này không đạt thì không công trình nào có thể đáp ứng được yêu cầu. 
2. Nếu$n \ge m$, ngay lập tức xuất ra “KHÔNG”. Đây là hệ quả trực tiếp của nguyên lý chuồng chim áp dụng cho các tổng tiền tố modulo$m$. Không sắp xếp các giá trị trong$[1, m]$có thể tránh lặp lại trạng thái modulo. 
3. Nếu$n < m$, xây dựng mảng bằng cách đặt mọi phần tử thành$1$. Điều này tạo ra tổng tiền tố$s_i = i$, đang tăng nghiêm ngặt dưới dạng số nguyên. 
4. Vì$s_i = i$, tất cả các tổng tiền tố đều có modulo riêng biệt$m$miễn là không có hai số nguyên giữa$0$Và$n$khác nhau bởi bội số của$m$. Bởi vì$n < m$, điều này là không thể. 
5. Đầu ra “YES” theo sau là mảng đã xây dựng. 

### Tại sao nó hoạt động 

Bất biến chính là tổng tiền tố tăng đúng một đơn vị ở mỗi bước, do đó không có hai tổng tiền tố nào có thể trùng nhau dưới dạng số nguyên. Bởi vì đẳng thức modulo hàm ý hiệu số nguyên là bội số của$m$và tất cả sự khác biệt giữa các tổng tiền tố nằm ở$[1, n]$, không có sự khác biệt nào có thể đạt tới$m$. Điều này đảm bảo rằng không có tổng tiền tố nào lặp lại modulo$m$, tương đương với việc nói rằng không có tổng mảng con nào chia hết cho$m$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    n, m = map(int, input().split())
    
    if n >= m:
        out.append("NO")
        continue
    
    out.append("YES")
    out.append(" ".join(["1"] * n))

sys.stdout.write("\n".join(out))
```Việc thực hiện tuân theo chính xác cấu trúc của thuật toán. Việc kiểm tra tính khả thi`n >= m`là điểm quyết định duy nhất, xuất phát trực tiếp từ lập luận chuồng chim về dư lượng tiền tố. Việc xây dựng sử dụng một mảng liên tục những cái đó, giúp tránh mọi nguy cơ lặp lại mô-đun. 

Đầu ra được đệm để tránh lặp lại chi phí I/O trên tối đa$10^5$trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$(n, m) = (3, 5)$. Từ$3 < 5$, chúng ta xây dựng mảng. 

| tôi | một [tôi] | tổng tiền tố | mod 5 | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 
| 3 | 1 | 3 | 3 | 

Tất cả các phần dư tiền tố đều khác biệt, do đó không có tổng mảng con nào có thể chia hết cho 5. Điều này xác nhận tính đúng đắn cho một trường hợp hợp lệ điển hình. 

Bây giờ hãy xem xét$(n, m) = (5, 3)$. Từ$5 \ge 3$, chúng tôi từ chối ngay lập tức. 

| Bước | Hành động | Lý do | 
| --- | --- | --- | 
| 1 | Phát hiện$n \ge m$| Nhiều tiền tố hơn dư lượng | 
| 2 | Đầu ra KHÔNG | Vi phạm Pigeonhole không thể tránh khỏi | 

Điều này cho thấy trường hợp không thể xảy ra khi bất kỳ mảng nào cũng phải thất bại. 

Ví dụ đầu tiên chứng tỏ cách xây dựng tránh sự lặp lại mô-đun, trong khi ví dụ thứ hai cho thấy tính không thể xảy ra là do cấu trúc và không phụ thuộc vào việc lựa chọn các giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm xây dựng hoặc loại bỏ theo thời gian không đổi hoặc tuyến tính trên đầu ra | 
| Không gian |$O(1)$| Chỉ lưu trữ các biến trường hợp thử nghiệm hiện tại và bộ đệm đầu ra | 

Tổng công việc trên tất cả các trường hợp thử nghiệm tỷ lệ thuận với tổng số$n$, được giới hạn bởi$3 \cdot 10^5$, thoải mái trong giới hạn thời gian chạy 1 giây trong Python khi sử dụng đầu ra được đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        if n >= m:
            out.append("NO")
        else:
            out.append("YES")
            out.append(" ".join(["1"] * n))

    return "\n".join(out)

# provided samples (as interpreted from statement formatting)
assert run("3\n3 5\n3 4\n3 6\n") == "YES\n1 1 1\nYES\n1 1 1\nYES\n1 1 1"

# minimum case
assert run("1\n1 2\n") == "YES\n1"

# impossible small
assert run("1\n2 2\n") == "NO"

# boundary just possible
assert run("1\n2 3\n") == "YES\n1 1"

# large m, small n
assert run("1\n5 1000000\n") == "YES\n1 1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n < m | CÓ + những cái | tính đúng đắn mang tính xây dựng | 
| n = m | KHÔNG | thất bại ranh giới chặt chẽ | 
| n = 1 | CÓ | cấu trúc hợp lệ tối thiểu | 
| m lớn | CÓ | khả năng mở rộng và xây dựng thống nhất | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán ngay lập tức vượt qua kiểm tra tính khả thi nếu$m \ge 2$và xuất ra một mảng phần tử đơn`[1]`. Tổng tiền tố là`1`, đó là modulo khác 0$m$, do đó không có mảng con nào có thể vi phạm điều kiện. 

Khi$n = m$, thuật toán bác bỏ. Ví dụ, với$n = 3, m = 3$, bất kỳ mảng nào cũng tạo ra 4 tổng tiền tố trong đó có 0, nhưng chỉ tồn tại 3 lớp dư lượng. Việc theo dõi thủ công xác nhận việc lặp lại là không thể tránh khỏi bất kể giá trị được chọn là gì. 

Khi$m$là cực kỳ lớn so với$n$, chẳng hạn như$(n, m) = (10^5, 10^6)$, việc xây dựng sẽ tạo ra một chuỗi dài các số 1. Tổng tiền tố chạy từ 1 đến$10^5$, tất cả đều có modulo riêng biệt$m$, do đó điều kiện không đáng kể vì sự bao bọc không bao giờ xảy ra trong phạm vi.
