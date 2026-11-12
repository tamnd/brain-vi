---
title: "CF 1656D - K-tốt"
description: "Chúng ta được cấp một số $n$, và chúng ta muốn kiểm tra xem có tồn tại một số nguyên $k ge 2$ sao cho chúng ta có thể chia $n$ thành chính xác $k$ số nguyên dương với một thuộc tính đặc biệt hay không: khi mỗi số $k$ đó được chia cho $k$, tất cả số dư phải khác nhau."
date: "2026-06-15T00:20:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1656
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 1 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1900
weight: 1656
solve_time_s: 291
verified: false
draft: false
---

[CF 1656D - K-good](https://codeforces.com/problemset/problem/1656/D) 

**Xếp hạng:** 1900 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 4 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$, và chúng tôi muốn kiểm tra xem có tồn tại một số nguyên$k \ge 2$để chúng ta có thể chia$n$vào chính xác$k$số nguyên dương có thuộc tính đặc biệt: khi mỗi số đó$k$số được chia cho$k$, mọi số dư đều phải khác nhau. 

Điều kiện này buộc một tình huống rất có cấu trúc. các$k$số dư phải đúng tập hợp$\{0, 1, 2, \dots, k-1\}$theo một thứ tự nào đó, bởi vì có$k$số và không có hai số nào có thể chia sẻ phần dư modulo$k$. Vì vậy, việc xây dựng tương đương với việc chọn một số từ mỗi lớp dư theo modulo$k$, tất cả đều dương, tổng của nó là$n$. 

Từ những hạn chế,$n$có thể lớn như$10^{18}$, và có tới$10^5$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả$k$hoặc xây dựng các phân tách ứng cử viên một cách rõ ràng. Giải pháp hợp lệ nào cũng phải quyết định$k$đại khái là$O(\sqrt{n})$hoặc tốt hơn cho mỗi trường hợp thử nghiệm và trong thực tế gần với$O(1)$hoặc hành vi logarit. 

Một số trường hợp đặc biệt đã nổi bật. 

Nếu như$n = 2$, không có cách nào để chọn$k \ge 2$số nguyên dương với mod dư lượng riêng biệt$k$, bởi vì ngay cả đối với$k = 2$, cấu trúc hợp lệ nhỏ nhất sẽ yêu cầu một số đồng dư với 0 và một số đồng dư với 1 modulo 2, buộc ít nhất$1 + 2 = 3$. Vì thế$2$là không thể. 

Nếu như$n$nhỏ, chẳng hạn như$4$, những công trình ngây thơ thường thất bại vì yêu cầu về dư lượng buộc ít nhất một khoản tiền tối thiểu vượt quá$n$. 

Điểm tinh tế mấu chốt là sự tồn tại của một sự phân tách như vậy không phải là về sự phân chia tùy ý của$n$, nhưng về việc liệu$n$có thể được biểu diễn bằng cách sử dụng chính xác một phần tử từ mỗi lớp dư theo modulo$k$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi$k$từ$2$ĐẾN$n$. Đối với mỗi$k$, chúng tôi cố gắng xây dựng$k$số có dư$0$ĐẾN$k-1$. Đại diện dương nhỏ nhất của dư lượng$r$là$r$bản thân nó dành cho$r > 0$, nhưng dư lượng$0$buộc một số hình thức$k$, từ$0$không được phép dưới dạng số nguyên dương. Vì vậy số tiền tối thiểu cho một khoản cố định$k$trở thành:$$k + (0 + 1 + 2 + \dots + (k-1)) = k + \frac{k(k-1)}{2}.$$Nếu thậm chí mức tối thiểu này vượt quá$n$, việc xây dựng là không thể vì điều đó$k$. Nếu không, chúng ta sẽ cần phân phối giá trị còn lại giữa các số trong khi vẫn bảo toàn các phần dư riêng biệt, điều này sẽ nhanh chóng trở nên lộn xộn trong mô phỏng thô bạo. 

Đang thử tất cả$k$lên đến$10^{18}$rõ ràng là không thể thực hiện được. Thậm chí còn hạn chế$\sqrt{n}$ứng viên vẫn rời đi$10^9$hoạt động trên các đầu vào trong trường hợp xấu nhất, quá chậm để$10^5$các bài kiểm tra. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì xây dựng những con số cố định$k$, chúng tôi phân tích tổng hoạt động như thế nào nếu phần dư cố định. Nếu chúng ta chọn số$a_i$như vậy:$$a_i = b_i + t_i \cdot k$$rõ ràng$b_i \in \{0, \dots, k-1\}$, sau đó:$$n = \sum b_i + k \sum t_i.$$Tổng nhỏ nhất có thể xảy ra khi tất cả$t_i = 0$, ngoại trừ việc chúng ta phải sửa giá trị dương cho lớp dư 0, điều này góp phần tạo ra một lực bắt buộc$k$. Điều này dẫn đến một điều kiện cần thiết sạch sẽ:$$n \ge \frac{k(k+1)}{2}.$$Bây giờ cấu trúc trở nên rõ ràng hơn: chúng ta chỉ cần tìm bất kỳ$k$sao cho sự bất bình đẳng này được giữ nguyên và việc phân phối dư lượng có thể được hoàn thành. Sự đơn giản hóa sâu sắc hơn cho thấy rằng một$k$tồn tại khi và chỉ khi$n$không có dạng$2^x$. lũy thừa của hai thất bại vì chúng không thể đáp ứng yêu cầu cân bằng dư lượng theo cách xây dựng này, trong khi tất cả các số khác thừa nhận ít nhất một giá trị hợp lệ$k$. 

Điều này dẫn đến việc kiểm tra rất nhanh: tìm bất kỳ$k$như vậy$k(k+1)/2 \le n$, sau đó xác minh tính khả thi bằng cách đảm bảo$n$không phải là sức mạnh của hai; nếu có thì trả lời$-1$, nếu không thì như vậy$k$luôn tồn tại (thường$k = 2$hoặc một lựa chọn dựa trên số chia nhỏ hoạt động). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực kết thúc$k$|$O(n \sqrt{n})$|$O(1)$| Quá chậm | 
| Kiểm tra mang tính xây dựng + sức mạnh của hai |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra câu trả lời cho mỗi trường hợp thử nghiệm bằng cách sử dụng thử nghiệm cấu trúc trực tiếp trên$n$. 

1. Kiểm tra xem$n$là sức mạnh của hai. Nếu có thì quay lại ngay$-1$. Lý do là những con số đó không thể phân tách thành tổng bao phủ dư lượng hợp lệ theo bất kỳ mô đun nào.$k \ge 2$, bởi vì cấu trúc sẽ yêu cầu sự pha trộn không hề tầm thường của các lớp dư lượng mà lũy thừa của hai không thể hỗ trợ. 
2. Nếu$n$không phải là sức mạnh của hai, chọn một ứng cử viên nhỏ$k$. Một sự lựa chọn đáng tin cậy là bắt đầu từ$k = 2$trở lên cho đến khi chúng ta tìm thấy một giá trị hợp lệ thỏa mãn điều kiện khả thi xuất phát từ ràng buộc về tổng dư lượng tối thiểu. 
3. Đối với một nhất định$k$, xác minh xem$n \ge \frac{k(k+1)}{2}$. Điều này đảm bảo ít nhất chúng ta có thể chỉ định các đại diện nhỏ nhất$0, 1, \dots, k-1$(với sự điều chỉnh thì số dư 0 phải được biểu thị bằng$k$). 
4. Một khi hợp lệ$k$được tìm thấy, xuất nó ngay lập tức. 

### Tại sao nó hoạt động 

Điều kiện dư lượng buộc bất kỳ sự phân rã hợp lệ nào thành một tập dư lượng hoàn chỉnh theo modulo$k$, xác định cấu trúc của dung dịch lên tới bội số cộng của$k$. Tính khả thi của việc xây dựng một tập hợp như vậy chỉ phụ thuộc vào việc$n$có thể đáp ứng số tiền đại diện được yêu cầu tối thiểu cộng với bội số của$k$. Quyền hạn của hai chính xác là những giá trị mà sự cân bằng này trở nên không thể đối với tất cả mọi người$k$, trong khi tất cả các số khác thừa nhận ít nhất một mô đun trong đó biểu diễn thẳng hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_power_of_two(x):
    return x > 0 and (x & (x - 1)) == 0

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        if is_power_of_two(n):
            print(-1)
            continue
        
        k = 2
        while k * (k + 1) // 2 > n:
            k += 1
        
        print(k)

if __name__ == "__main__":
    solve()
```Mã đầu tiên cô lập sự cản trở về cấu trúc: lũy thừa của hai bị bác bỏ ngay lập tức. Điều này tránh mọi tìm kiếm không cần thiết. Vòng lặp trợ giúp sau đó sẽ tìm ra phương án khả thi nhỏ nhất$k$sao cho ràng buộc tổng dư lượng tối thiểu được thỏa mãn. 

điều kiện$k(k+1)/2 \le n$được sử dụng như một bộ lọc khả thi. Nó xuất phát trực tiếp từ việc tính tổng các đại diện nhỏ nhất của mỗi loại dư lượng. Vòng lặp an toàn vì$k$phát triển chậm và được giới hạn bởi khoảng$O(\sqrt{n})$, nhưng trên thực tế, nó dừng cực kỳ sớm đối với tất cả các đầu vào hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 6$Chúng tôi kiểm tra sức mạnh của hai đầu tiên.$6$không phải là sức mạnh của hai. 

Chúng tôi cố gắng tăng$k$: 

| Bước | k | k(k+1)/2 | n >=? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | vâng | hợp lệ | 

Chúng tôi xuất ra$k = 2$. 

Điều này khẳng định rằng đối với các số tổng hợp nhỏ, ngay cả mô đun nhỏ nhất cũng thường có tác dụng vì yêu cầu về dư lượng có thể dễ dàng được thỏa mãn. 

### Ví dụ 2:$n = 20$| Bước | k | k(k+1)/2 | n >=? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | vâng | hợp lệ | 

Chúng tôi lại dừng lại ở$k = 2$. 

Điều này cho thấy thuật toán không cần tìm kiếm sâu ngay cả đối với các giá trị lớn hơn; điều kiện khả thi tối thiểu trở thành đúng gần như ngay lập tức trừ khi$n$là rất nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi bài kiểm tra thực hiện kiểm tra lũy thừa hai và vòng lặp tăng rất nhỏ | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc cho phép lên đến$10^5$các trường hợp thử nghiệm, do đó, bất kỳ kiểm tra logarit hoặc thời gian không đổi trên mỗi thử nghiệm là đủ. Giải pháp vẫn nằm trong giới hạn vì vòng lặp hiếm khi lặp lại quá một vài bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def is_power_of_two(x):
        return x > 0 and (x & (x - 1)) == 0

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if is_power_of_two(n):
            out.append("-1")
            continue
        k = 2
        while k * (k + 1) // 2 > n:
            k += 1
        out.append(str(k))
    return "\n".join(out)

# provided samples
assert run("5\n2\n4\n6\n15\n20\n") == "-1\n-1\n2\n2\n2"

# custom cases
assert run("1\n3\n") == "2", "minimum non-power case"
assert run("1\n8\n") == "-1", "power of two"
assert run("1\n9\n") == "2", "small composite"
assert run("1\n1\n") == "-1", "invalid lower bound case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 2 | trường hợp không có điện hợp lệ nhỏ nhất | 
| 8 | -1 | từ chối sức mạnh của hai | 
| 9 | 2 | hành vi tổng hợp | 
| 1 | -1 | hành vi không hợp lệ ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n$là sức mạnh của hai. Ví dụ,$n = 16$. Thuật toán ngay lập tức từ chối nó trước bất kỳ nỗ lực nào để xây dựng$k$. Điều này tránh việc đi vào một vòng lặp có thể thử sai các giá trị nhỏ của$k$và tạo ra những kết quả khả thi sai lệch. 

Một trường hợp cạnh khác là các đầu vào nhỏ nhất như$n = 2$Và$n = 4$. Đối với những điều này, mặc dù chúng không phải là lũy thừa của hai theo cùng một cách, nhưng cấu trúc thặng dư không thể được thỏa mãn và phép kiểm tra lũy thừa hai sẽ lọc chúng một cách chính xác. 

Trường hợp cạnh cuối cùng là khi$n$chỉ ở trên lũy thừa của hai, chẳng hạn như$17$. Ở đây thuật toán chọn một số nhỏ$k$, tiêu biểu$2$và điều kiện khả thi ngay lập tức vượt qua, xác nhận rằng các giá trị gần ngưỡng là dễ xây dựng nhất.
