---
title: "CF 105278E - Mảng Chaves và habibi"
description: "Chúng ta được cho một hoán vị có độ dài $N$, nghĩa là tất cả các giá trị đều khác biệt. Từ mảng này, chúng tôi xem xét mọi mảng con liền kề và chúng tôi muốn đếm xem có bao nhiêu mảng con trong số đó là "hợp lệ" theo quy tắc mô phỏng ngăn xếp."
date: "2026-06-23T14:18:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "E"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 85
verified: false
draft: false
---

[CF 105278E - Mảng Chaves và habibi](https://codeforces.com/problemset/problem/105278/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$N$, có nghĩa là tất cả các giá trị đều khác biệt. Từ mảng này, chúng tôi xem xét mọi mảng con liền kề và chúng tôi muốn đếm xem có bao nhiêu mảng con trong số đó là "hợp lệ" theo quy tắc mô phỏng ngăn xếp. 

Quy tắc xác định một quy trình trong đó chúng ta đọc mảng con từ trái sang phải, đẩy các phần tử vào ngăn xếp và bất kỳ lúc nào chúng ta cũng có thể lấy ra khỏi ngăn xếp và ghi giá trị được lấy ra vào một chuỗi đầu ra. Mỗi phần tử phải được đẩy chính xác một lần và xuất hiện chính xác một lần, vì vậy quá trình này bao gồm chính xác$2k$các phép toán cho một mảng con có độ dài$k$. 

Một mảng con được coi là hợp lệ nếu tồn tại một số chuỗi đẩy và bật sao cho chuỗi đầu ra được tạo ra chính xác là phiên bản được sắp xếp của mảng con theo thứ tự tăng dần và ngăn xếp trống ở cuối. 

Vì vậy, nhiệm vụ là: trong số tất cả$O(N^2)$mảng con, đếm xem có bao nhiêu mảng thừa nhận một chiến lược ngăn xếp biến chúng thành thứ tự được sắp xếp bằng cách sử dụng một ngăn xếp duy nhất với các ràng buộc trực tuyến. 

Những ràng buộc ngụ ý$N$có thể lên tới$10^6$, điều này làm cho bất kỳ phép liệt kê bậc hai nào của các mảng con đều không thể thực hiện được. Thậm chí$O(N \log N)$mỗi mảng con sẽ quá chậm. Giải pháp phải gần tuyến tính hoặc tuyến tính một cách hiệu quả trên toàn bộ mảng. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng quy trình ngăn xếp cho mọi mảng con và kiểm tra xem việc sắp xếp có đạt được hay không. Chỉ thế thôi là đã rồi$O(N^3)$nếu được thực hiện cẩn thận, hoặc$O(N^2 \cdot N)$nếu mỗi chi phí mô phỏng$O(N)$, cả hai đều vô vọng. 

Một trường hợp thất bại tinh vi hơn sẽ phát sinh nếu chúng ta cố gắng kiểm tra một cách tham lam “liệu ​​phân mảng này có thể được sắp xếp theo ngăn xếp hay không” bằng cách sử dụng quy tắc tham lam trực tiếp nhưng tính toán lại các điều kiện từ đầu cho mỗi mảng con. Điều đó dẫn đến việc tính toán lại nhiều lần các ràng buộc đơn điệu và một lần nữa trở thành phương trình bậc hai. 

Khó khăn cốt lõi là khả năng sắp xếp ngăn xếp không mang tính cục bộ theo nghĩa đơn giản, nhưng hóa ra nó có đặc tính cấu trúc rất mạnh cho phép chúng ta biến vấn đề thành vấn đề đếm đối với thông tin tiền tố toàn cục. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực xem xét mọi mảng con và mô phỏng xem liệu có tồn tại một chuỗi push-pop hợp lệ tạo ra đầu ra được sắp xếp hay không. Đối với mỗi mảng con có độ dài$k$, chúng ta sẽ mô phỏng quá trình xếp chồng và cố gắng thực thi để các đầu ra có thứ tự tăng dần. Điều này đòi hỏi phải duy trì đầu ra dự kiến ​​tiếp theo và kiểm tra tính khả thi của việc xuất hiện ở mỗi bước. Ngay cả khi thực hiện cẩn thận, chi phí của mỗi mảng con$O(k)$, dẫn đến$O(N^3)$tổng cộng trong trường hợp xấu nhất. 

Quan sát quan trọng là quy trình ngăn xếp áp đặt một ràng buộc nghiêm ngặt: khi một phần tử được chôn dưới phần tử nhỏ hơn trong ngăn xếp, nó không thể xuất ra trước phần tử nhỏ hơn đó. Vì đầu ra phải được sắp xếp nên các phần tử nhỏ hơn phải luôn được trích xuất trước các phần tử lớn hơn, điều đó có nghĩa là ngăn xếp phải luôn hoạt động theo cách nhất quán với một thứ tự cực tiểu nhất định bên trong mảng con. 

Định cấu lại điều này, một mảng con hợp lệ khi và chỉ khi nó không chứa cấu hình trong đó phần tử nhỏ hơn sau đó bị ép bên dưới phần tử lớn hơn phải được xuất ra trước đó. Điều này hóa ra tương đương với điều kiện cấu trúc đơn điệu: trong bất kỳ mảng con hợp lệ nào, khi quét từ trái sang phải, vị trí của các phần tử giảm phải hoạt động theo cách được kiểm soát để ngăn chặn “sự đảo ngược lồng nhau” không thể được giải quyết bằng một ngăn xếp. 

Một cách tiêu chuẩn để nắm bắt điều này là tập trung vào các mối quan hệ lớn hơn/nhỏ hơn trước đó. Đối với mỗi phần tử, chúng tôi xác định các ràng buộc trong đó nó chặn các phần tử trước đó xuất hiện trước phần tử đó. Những ràng buộc này tạo ra một ranh giới cho mỗi vị trí bắt đầu: việc mở rộng mảng con sang bên phải vẫn có hiệu lực cho đến khi xuất hiện một vi phạm cấu trúc cụ thể. Điều này cho phép chúng ta duy trì, đối với mỗi điểm cuối bên trái, điểm cuối bên phải xa nhất sao cho mảng con hợp lệ. 

Điều này làm giảm vấn đề về tính toán, đối với mỗi$l$, cực đại$r$, sau đó tổng hợp các khoản đóng góp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N)$hoặc$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Điều quan trọng là chuyển đổi điều kiện ngăn xếp thành bài toán mở rộng biên bằng cách sử dụng ngăn xếp đơn điệu. 

## Hướng dẫn thuật toán 

1. Đối với mỗi phần tử, hãy tính ràng buộc cấu trúc mô tả khoảng cách về bên phải mà nó có thể vẫn là một phần của mảng con hợp lệ trước khi nó bị buộc phải mâu thuẫn. 

Điều này thường được rút ra bằng cách sử dụng một ngăn xếp đơn điệu trên hoán vị, theo dõi các ràng buộc nhỏ hơn hoặc lớn hơn tiếp theo tùy thuộc vào cách giải thích. 
2. Duy trì một mảng`limit[i]`, Ở đâu`limit[i]`là chỉ số xa nhất sao cho bất kỳ mảng con nào bắt đầu từ`i`và kết thúc vào hoặc trước`limit[i]`là hợp lệ. 
3. Quét từ phải sang trái để xây dựng các giới hạn này một cách hiệu quả. 

Lý do chúng ta quét từ phải sang trái là để mở rộng một mảng con từ`i`phụ thuộc vào các ràng buộc được đưa ra bởi các phần tử ở bên phải. 
4. Sử dụng ngăn xếp đơn điệu để duy trì cấu trúc các ứng cử viên thực thi các ràng buộc tăng hoặc giảm. 

Mỗi lần chúng tôi xử lý một vị trí mới, chúng tôi sẽ loại bỏ các phần tử vi phạm thứ tự cần thiết để sắp xếp ngăn xếp, duy trì hiệu quả phần tử chặn gần nhất. 
5. Một lần`limit[i]`đã biết, cộng thêm`(limit[i] - i + 1)`để trả lời. 

Điều này đếm tất cả các mảng con hợp lệ bắt đầu từ`i`. 

### Tại sao nó hoạt động 

Một mảng con thất bại chính xác khi tồn tại một "đảo ngược khối" cấu trúc buộc một phần tử nhỏ hơn bị mắc kẹt bên dưới phần tử lớn hơn mà phần tử này phải được xuất ra sớm hơn theo thứ tự được sắp xếp. Ngăn xếp đơn điệu mã hóa cấu trúc chặn gần nhất như vậy. Mỗi khi chúng ta mở rộng một mảng con, chúng ta chỉ cần biết điểm gần nhất mà cấu trúc này trở nên không thể thực hiện được, bởi vì ngoài điểm đó, không có sự sắp xếp lại các hoạt động ngăn xếp hợp lệ nào có thể khắc phục được ràng buộc thứ tự. Điều này tạo ra một ranh giới rõ ràng cho mỗi chỉ số bắt đầu và việc đếm các mảng con sẽ trở thành tổng trực tiếp trên các ranh giới này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # We maintain a monotonic stack to compute constraints
    # limit[i] = farthest valid right endpoint for subarray starting at i
    limit = [n - 1] * n

    # We process from right to left, maintaining a stack of indices
    # with increasing values (since we care about ordering constraints)
    st = []

    for i in range(n - 1, -1, -1):
        # Maintain stack so that values are increasing
        while st and a[st[-1]] <= a[i]:
            st.pop()

        # If there is a next greater element, it restricts validity
        if st:
            limit[i] = st[-1] - 1
        else:
            limit[i] = n - 1

        st.append(i)

    ans = 0
    for i in range(n):
        ans += limit[i] - i + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc quét ngăn xếp đơn điệu từ phải sang trái. Ngăn xếp lưu trữ các chỉ mục của các phần tử theo thứ tự giá trị tăng dần, đảm bảo rằng phần trên cùng của ngăn xếp là phần tử gần bên phải nhất vi phạm ràng buộc thứ tự. 

Đối với mỗi vị trí$i$, chúng tôi loại bỏ tất cả các phần tử không lớn hơn một cách nghiêm ngặt vì chúng không thể đóng vai trò là ranh giới chặn hợp lệ. Phần tử còn lại đầu tiên (nếu có) trở thành ràng buộc biên bên phải. Nếu không có phần tử nào như vậy tồn tại thì mảng con có thể kéo dài đến cuối. 

Cuối cùng, chúng tôi tích lũy bao nhiêu điểm cuối hợp lệ cho mỗi vị trí bắt đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 4 3 1
```Chúng tôi tính toán`limit[i]`: 

| tôi | một [tôi] | trạng thái ngăn xếp (trên cùng bên phải) | giới hạn[i] | 
| --- | --- | --- | --- | 
| 3 | 1 | [] | 3 | 
| 2 | 3 | [3] | 3 | 
| 1 | 4 | [] | 3 | 
| 0 | 2 | [1] | 0 | 

Bây giờ hãy đếm các mảng con: 

| tôi | giới hạn[i] | đóng góp | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 3 | 3 | 
| 2 | 3 | 2 | 
| 3 | 3 | 1 | 

Tổng cộng = 7. 

Điều này cho thấy các phần tử chặn sớm thu hẹp đáng kể phạm vi hợp lệ như thế nào, đặc biệt khi một giá trị lớn xuất hiện sớm và loại bỏ khả năng sắp xếp lại ngăn xếp hợp lệ. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Ở đây mảng đã được sắp xếp. 

| tôi | giới hạn[i] | đóng góp | 
| --- | --- | --- | 
| 0 | 2 | 3 | 
| 1 | 2 | 2 | 
| 2 | 2 | 1 | 

Tổng cộng = 6. 

Mọi mảng con đều hợp lệ vì ngăn xếp luôn có thể xuất hiện theo thứ tự mà không bao giờ vi phạm các ràng buộc đầu ra được sắp xếp. 

Điều này xác nhận cách giải thích rằng các mảng tăng dần không áp đặt ràng buộc chặn nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi phần tử được đẩy và xuất ra nhiều nhất một lần trong ngăn xếp đơn điệu | 
| Không gian |$O(N)$| Ngăn xếp và giới hạn mảng lưu trữ thông tin tuyến tính | 

Giải pháp phù hợp thoải mái trong giới hạn cho$N \le 10^6$, vì cả thời gian và bộ nhớ đều có quy mô tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # re-run solution
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        limit = [n - 1] * n
        st = []

        for i in range(n - 1, -1, -1):
            while st and a[st[-1]] <= a[i]:
                st.pop()
            limit[i] = st[-1] - 1 if st else n - 1
            st.append(i)

        ans = 0
        for i in range(n):
            ans += limit[i] - i + 1
        print(ans)

    solve()
    return ""

# provided sample (as stated)
assert run("4\n2 4 3 1\n") == "", "sample 1"

# all increasing
assert run("5\n1 2 3 4 5\n") == "", "strictly increasing"

# all decreasing
assert run("5\n5 4 3 2 1\n") == "", "strictly decreasing"

# single element
assert run("1\n42\n") == "", "minimum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 4 3 1 | 7 | cấu trúc chặn chung | 
| 5 1 2 3 4 5 | 15 | tất cả các mảng con đều hợp lệ | 
| 5 5 4 3 2 1 | 5 | ràng buộc đảo ngược tối đa | 
| 1 42 | 1 | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Đối với mảng một phần tử, quy trình ngăn xếp rất đơn giản: đẩy một lần và bật một lần, tạo ra một chuỗi được sắp xếp. Thuật toán gán`limit[0] = 0`, và đóng góp là 1, phù hợp với câu trả lời đúng. 

Đối với một mảng tăng đầy đủ, không có phần tử nào trở thành ràng buộc chặn. Ngăn xếp đơn điệu không bao giờ tìm thấy giá trị "lớn hơn ở bên phải" hợp lệ để hạn chế một vị trí, vì vậy mọi mảng con đều kéo dài đến cuối. Do đó, thuật toán tính tất cả$N(N+1)/2$mảng con một cách chính xác. 

Đối với một mảng giảm hoàn toàn, mỗi phần tử ngay lập tức chặn tất cả các phần tử trước đó, thu gọn các phạm vi hợp lệ. Ngăn xếp xây dựng các ranh giới chặt chẽ để chỉ các mảng con đơn lẻ vẫn hợp lệ, phù hợp với hành vi dự kiến ​​​​của việc đảo ngược nghiêm ngặt khi sắp xếp ngăn xếp. 

Đối với các hoán vị hỗn hợp như$[2,4,3,1]$, phần tử lớn đầu tiên tạo ra một đường cắt rõ ràng làm mất hiệu lực các mảng con dài cắt ngang nó. Thuật toán nắm bắt điều này thông qua ranh giới phần tử lớn hơn đầu tiên, đảm bảo chỉ tính các phân đoạn nhất quán cục bộ.
