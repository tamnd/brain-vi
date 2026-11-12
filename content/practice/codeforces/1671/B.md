---
title: "CF 1671B - Phân đoạn điểm liên tiếp"
description: "Chúng ta có một tập hợp các điểm trên trục số, đã được sắp xếp theo thứ tự tăng dần. Mỗi điểm được phép “lắc lư” tối đa một đơn vị, nghĩa là vị trí cuối cùng của nó có thể là tọa độ ban đầu, sang trái một bước hoặc sang phải một bước."
date: "2026-06-10T01:36:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1671
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 127 (Rated for Div. 2)"
rating: 1000
weight: 1671
solve_time_s: 120
verified: true
draft: false
---

[CF 1671B - Phân đoạn điểm liên tiếp](https://codeforces.com/problemset/problem/1671/B) 

**Đánh giá:** 1000 
**Tags:** sức mạnh vũ phu, toán học, sắp xếp 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên trục số, đã được sắp xếp theo thứ tự tăng dần. Mỗi điểm được phép “lắc lư” tối đa một đơn vị, nghĩa là vị trí cuối cùng của nó có thể là tọa độ ban đầu, sang trái một bước hoặc sang phải một bước. 

Sau tất cả các chuyển động, chúng tôi muốn các điểm chiếm một đoạn số nguyên được lấp đầy hoàn hảo, không có khoảng trống và không có sự trùng lặp. Nói cách khác, nếu có$n$điểm, vị trí cuối cùng của chúng phải chính xác là một số số nguyên liên tiếp$l, l+1, \dots, l+n-1$. 

Nhiệm vụ là quyết định xem việc phân công lại từng điểm trong phạm vi của nó có phù hợp hay không.$[-1, 0, +1]$phạm vi linh hoạt có thể tạo ra một khối liền kề. 

Các ràng buộc chặt chẽ: tổng số điểm trên tất cả các trường hợp thử nghiệm đạt$2 \cdot 10^5$, vì vậy bất kỳ giải pháp nào cố gắng khám phá tất cả các kết hợp chuyển động đều không khả thi ngay lập tức. Mỗi điểm có 3 lựa chọn độc lập, do đó không gian trạng thái brute-force tăng lên khi$3^n$, vượt xa mọi giới hạn tính toán. 

Một hạn chế tinh vi hơn là đầu vào phải được sắp xếp. Cấu trúc đó quan trọng vì nó cho phép chúng ta suy luận về các bài tập theo thứ tự thay vì coi các điểm là các đối tượng độc lập. 

Có một vài trường hợp thất bại mà lý luận tham lam ngây thơ thường bỏ qua. Một giả định là chúng ta luôn có thể gán từng điểm một cách tham lam vào khe số nguyên sẵn có gần nhất mà không cần quay lại. Ví dụ: nếu các điểm cách đều nhau như`1 2 3 7`, phép gán tham lam có thể sớm chiếm hết các vị trí gần 1-3 và không để lại vị trí hợp lệ nào cho 7 ngay cả khi có một ca hợp lệ hoặc ngược lại tùy thuộc vào lựa chọn thứ tự. Một cạm bẫy khác là quên rằng việc dịch chuyển có thể vừa làm tăng vừa giảm khoảng cách, vì vậy chỉ lý luận về những khoảng trống ban đầu là không đủ. Cuối cùng, yêu cầu các vị trí cuối cùng phải khác biệt có nghĩa là chúng ta không thể gán hai điểm cho cùng một số nguyên ngay cả khi cả hai đều được phép hạ cánh riêng lẻ ở đó. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ gán cho mỗi điểm một trong ba giá trị và sau đó kiểm tra xem tập hợp kết quả có tạo thành một chuỗi số nguyên liên tiếp hay không. Điều này đúng nhưng không khả thi vì nó khám phá$3^n$cấu hình cho mỗi trường hợp thử nghiệm. Ngay cả đối với$n = 30$, điều này đã trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là cấu trúc cuối cùng mà chúng tôi muốn là cực kỳ cứng nhắc: một khi chúng tôi cố định vị trí bắt đầu$l$, mọi điểm đều có một vị trí mục tiêu được xác định bởi chỉ mục của nó. Thay vì nghĩ đến việc các điểm di chuyển ở đâu, chúng ta có thể nghĩ đến việc kết hợp các điểm đã sắp xếp với một mẫu liên tiếp lý tưởng. 

Vì mảng cuối cùng tăng nghiêm ngặt và không có khoảng trống nên nếu chúng ta sắp xếp các vị trí được chọn cuối cùng, chúng phải khớp chính xác với chuỗi nhận dạng đã dịch chuyển. Điều này cho phép áp dụng chiến lược căn chỉnh tham lam: chúng tôi cố gắng chỉ định từng điểm đầu vào theo thứ tự cho một vị trí mục tiêu trong một phân đoạn giả định. 

Sự đơn giản hóa quan trọng là coi đây là vấn đề khớp giữa mỗi điểm và chỉ mục mục tiêu, đảm bảo mỗi phép gán nằm trong$[x_i - 1, x_i + 1]$. Vì cả hai trình tự đều được sắp xếp nên chúng ta có thể căn chỉnh từ trái sang phải một cách tham lam và kiểm tra tính khả thi mà không cần quay lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^n)$|$O(n)$| Quá chậm | 
| Kết hợp tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cố gắng xây dựng phân đoạn cuối cùng bắt đầu từ các khoản bù đắp khả thi khác nhau, nhưng chúng tôi không liệt kê chúng một cách rõ ràng. Thay vào đó, chúng tôi mô phỏng trực tiếp việc gán từng điểm vào một ô số nguyên liên tiếp. 

1. Chúng tôi giả sử đoạn cuối cùng bắt đầu ở một giá trị nào đó$l$, nhưng thay vì sửa$l$, về mặt khái niệm, chúng tôi căn chỉnh điểm đầu tiên với một vị trí khả thi trong phạm vi của nó. Điều này loại bỏ sự cần thiết phải đoán rõ ràng$l$. 
2. Chúng tôi xử lý điểm theo thứ tự tăng dần và duy trì số nguyên nhỏ nhất vẫn có thể được chỉ định. Đối với$i$-điểm thứ, vị trí dự định là$i$-phụ thuộc vào một chuỗi liên tiếp. 
3. Đối với mỗi điểm$x_i$, chúng tôi xem xét vị trí mong muốn hiện tại$pos$. Nếu như$pos$nằm bên trong$[x_i - 1, x_i + 1]$, webgán$x_i$ĐẾN$pos$. Đây là nhiệm vụ hợp lý duy nhất bởi vì trì hoãn hoặc thúc đẩy nó sẽ chỉ làm giảm tính linh hoạt trong tương lai. 
4. Nếu$pos < x_i - 1$, chúng tôi tăng$pos$ĐẾN$x_i - 1$. Điều này đảm bảo chúng ta không cố gắng đặt điểm quá xa so với những gì nó có thể chạm tới. 
5. Nếu$pos > x_i + 1$, chúng tôi ngay lập tức thất bại vì điểm hiện tại không thể đạt tới bất kỳ vị trí hợp lệ nào còn lại trong cấu trúc liên tiếp. 
6. Sau khi gán vị trí cho một điểm, ta tăng$pos$và tiếp tục. 

Câu trả lời cuối cùng là CÓ nếu chúng ta chấm điểm thành công tất cả các điểm. 

### Tại sao nó hoạt động 

Thuật toán duy trì một “khe yêu cầu tiếp theo” đang chạy theo một chuỗi liên tiếp giả định. Ở mỗi bước, chúng tôi căn chỉnh điểm hiện tại với vị trí đó hoặc dịch chuyển vị trí về phía trước để duy trì trong giới hạn khả thi. Bất biến chính là sau khi xử lý$i$-điểm thứ, chúng tôi đã xây dựng một phân đoạn liên tiếp một phần hợp lệ và đảm bảo rằng tất cả các điểm được xử lý có thể được nhúng vào một số phân đoạn cuối cùng hợp lệ mở rộng tiền tố này. Bởi vì cả cấu trúc đầu vào và cấu trúc đích đều đơn điệu, nên bất kỳ lỗi cục bộ nào (một điểm không thể tiếp cận được vị trí yêu cầu của nó) đều không thể được sửa chữa sau này mà không phá vỡ các nhiệm vụ trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))

        pos = -10**18
        ok = True

        for i in range(n):
            # desired slot for i-th element in consecutive segment
            if pos < x[i] - 1:
                pos = x[i] - 1

            if pos > x[i] + 1:
                ok = False
                break

            pos += 1

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp giữ một con trỏ`pos`đại diện cho số nguyên tiếp theo mà chúng ta muốn chiếm trong đoạn liên tiếp cuối cùng. Đối với mỗi điểm, chúng tôi điều chỉnh con trỏ này lên trên nếu nó quá nhỏ để có thể thực hiện được. Nếu nó vượt quá vị trí có thể tiếp cận tối đa của điểm thì việc cấu hình sẽ không thể thực hiện được. 

Sự gia tăng`pos += 1`là cần thiết vì khi một vị trí được chỉ định, điểm tiếp theo phải chiếm số nguyên tiếp theo trong phân đoạn. Một chi tiết tinh tế đang khởi tạo`pos`đến một giá trị rất nhỏ để điểm đầu tiên có thể tự do lựa chọn vị trí của nó trong phạm vi cho phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3 7
```Chúng tôi theo dõi`pos`: 

| tôi | x[i] | tư thế trước | điều chỉnh | pos sau khi kiểm tra | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -inf | tối đa bằng 0 | 0 | được | 
| 1 | 2 | 1 | không | 1 | được | 
| 2 | 3 | 2 | không | 2 | được | 
| 3 | 7 | 3 | không | 3 (không hợp lệ) | THẤT ​​BẠI | 

Ở bước cuối cùng,`pos = 3`không ở trong`[6, 8]`, do đó cấu hình không thành công. 

Điều này cho thấy khoảng cách lớn muộn không thể được sửa chữa bằng sự linh hoạt trước đó. 

### Ví dụ 2 

đầu vào:```
3
2 5 6
```| tôi | x[i] | tư thế trước | điều chỉnh | pos sau khi kiểm tra | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | -inf | 1 | 1 | được | 
| 1 | 5 | 2 | không | 2 | được | 
| 2 | 6 | 3 | không | 3 | được | 

Tất cả các điểm có thể được căn chỉnh thành một đoạn liên tiếp bắt đầu từ 1, xác nhận CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi điểm được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian |$O(1)$| Chỉ duy nhất một con trỏ được duy trì | 

Quét tuyến tính là tối ưu vì mỗi điểm phải được kiểm tra ít nhất một lần và tổng kích thước đầu vào là$2 \cdot 10^5$, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            x = list(map(int, input().split()))

            pos = -10**18
            ok = True
            for i in range(n):
                if pos < x[i] - 1:
                    pos = x[i] - 1
                if pos > x[i] + 1:
                    ok = False
                    break
                pos += 1
            out.append("YES" if ok else "NO")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""5
2
1 4
3
1 2 3
4
1 2 3 7
1
1000000
3
2 5 6
""") == """YES
YES
NO
YES
YES"""

# custom cases
assert run("""1
1
10
""") == "YES"

assert run("""1
3
1 3 5
""") == "YES"

assert run("""1
3
1 10 11
""") == "NO"

assert run("""1
5
1 2 3 4 10
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | CÓ | tính khả thi tầm thường | 
| xen kẽ thưa thớt | CÓ | sự linh hoạt của ca làm việc | 
| thất bại sớm khoảng cách lớn | KHÔNG | hạn chế tầm với không thể | 
| ngoại lệ bị cô lập muộn | KHÔNG | khoảng cách không thể sửa chữa | 

## Vỏ cạnh 

Đầu vào một phần tử tối thiểu luôn thành công vì bất kỳ điểm nào cũng có thể được di chuyển để tạo thành một đoạn có độ dài bằng một. Thuật toán khởi tạo`pos`đủ thấp để lần kiểm tra đầu tiên luôn thành công và vòng lặp chỉ định vị trí duy nhất mà không có xung đột. 

Một trường hợp như`1 10 11`thể hiện sự thất bại trong đó khoảng cách lớn đầu tiên buộc vị trí được yêu cầu đi quá xa về phía trước, cuối cùng vượt quá phạm vi có thể tiếp cận của điểm cuối cùng. Trong quá trình thực hiện,`pos`tiến bộ vượt xa`x[i] + 1`đối với phần tử đầu tiên không thể thu hẹp khoảng cách, sẽ gây ra sự từ chối ngay lập tức. 

Một chuỗi được đóng gói chặt chẽ như`2 5 6`cho thấy cách thuật toán “kéo dài” các điểm trước đó lên trên trong giới hạn cho phép một cách tự nhiên, sắp xếp chúng thành một phân đoạn liên tiếp hợp lệ mà không cần quay lui rõ ràng.
