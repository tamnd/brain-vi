---
title: "CF 105230E - Sản phẩm tuyệt vời"
description: "Chúng ta có một số nguyên $n$, và chúng ta muốn biểu thị nó dưới dạng tích của các số nguyên lớn hơn 1. Điều khó khăn là trong số tất cả các phép phân tích nhân tử có thể có, chúng ta không tối ưu hóa sự đơn giản hoặc số lượng số hạng tối thiểu, mà ngược lại: chúng ta muốn tối đa hóa số lượng thừa số…"
date: "2026-06-24T15:59:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "E"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 69
verified: true
draft: false
---

[CF 105230E - Sản phẩm tuyệt vời](https://codeforces.com/problemset/problem/105230/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng tôi muốn biểu thị nó dưới dạng tích của các số nguyên lớn hơn 1. Điều khó khăn là trong số tất cả các hệ số có thể có, chúng tôi không tối ưu hóa sự đơn giản hoặc số lượng số hạng tối thiểu mà ngược lại: chúng tôi muốn tối đa hóa số lượng yếu tố xuất hiện trong tích. 

Mỗi thừa số phải có ít nhất 2, vì việc sử dụng 1 bị cấm rõ ràng mặc dù nó sẽ cho phép biểu diễn độ dài vô hạn một cách tầm thường. Đầu ra là một chuỗi các số nguyên theo thứ tự không giảm có tích bằng$n$và trong số tất cả các phân tách hợp lệ, chúng tôi chọn phân tách có số lượng số hạng tối đa. 

Ràng buộc$2 \le n \le 10^5$đủ nhỏ để chúng ta có thể sử dụng các phương pháp phân tích thành thừa số$n$đại khái là$O(\sqrt{n})$, hoặc thậm chí$O(n)$ý tưởng tính toán trước. Điều chúng ta không thể thực hiện được là liệt kê tất cả các phân tích nhân tử hoặc thực hiện phép đệ quy hàm mũ trên các phân chia của các phân số, vì số lượng phân tách nhân tăng rất nhanh ngay cả đối với các giá trị vừa phải. 

Trường hợp cạnh chính là khi$n$là nguyên tố. Trong tình huống đó, không có hệ số hóa không tầm thường nào tồn tại, vì vậy biểu diễn hợp lệ duy nhất là chính số đó. 

Một trường hợp tinh tế khác xuất hiện khi tồn tại nhiều đường dẫn phân tích nhân tử nhưng khác nhau về độ sâu. Ví dụ,$12$có thể được viết là$3 \times 4$hoặc$2 \times 6$, nhưng chỉ có sự phân rã tiếp tục phân tách cho đến khi tất cả các thừa số nguyên tố mới đạt được độ dài tối đa. Một lựa chọn tham lam ngây thơ như luôn lấy ước số nhỏ nhất trước không tự động đảm bảo số lượng thừa số toàn cục tốt nhất trừ khi chúng ta chứng minh điều đó một cách hợp lý. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử đệ quy mọi phép chia có thể của$n$vào trong$a \times b$, sau đó tiếp tục phân tích cả hai thành nhân tử$a$Và$b$, theo dõi quá trình phân rã với số lượng số hạng tối đa. Điều này đúng vì nó khám phá toàn bộ không gian trạng thái của các phân vùng nhân. Tuy nhiên, ngay cả đối với mức độ vừa phải$n$, điều này trở nên không thể thực hiện được. Nhiều số có hàng chục ước số và mỗi nhánh lại dẫn đến sự phân nhánh tiếp theo, tạo ra sự bùng nổ theo cấp số nhân. Trong trường hợp xấu nhất, chẳng hạn như các số có tổng hợp cao, cây đệ quy sẽ phát triển cực kỳ lớn. 

Quan sát quan trọng là chúng ta không được yêu cầu các thừa số tùy ý mà yêu cầu phân tích để tối đa hóa số lượng số hạng. Để tối đa hóa số lượng các thừa số, chúng ta muốn chia các số càng nhiều càng tốt và việc chia bất kỳ hợp số nào thành các thừa số nhỏ hơn luôn làm tăng hoặc bảo toàn tổng số các thừa số. Điều này gợi ý rằng chiến lược tối ưu là phân hủy$n$hoàn toàn thành thừa số nguyên tố, vì số nguyên tố là đơn vị tối thiểu không thể phân chia được trong phép nhân. 

Một khi chúng ta đạt đến hệ số nguyên tố, thì không thể phân chia thêm được nữa, vì vậy số lượng các thừa số được tối đa hóa một cách chính xác khi mọi hợp số được chia nhỏ hoàn toàn. Điều này biến bài toán thành hệ số nguyên tố tiêu chuẩn, sau đó sắp xếp các số nguyên tố thu được theo thứ tự không giảm, điều này đã được thỏa mãn một cách tự nhiên nếu chúng ta thu thập chúng theo thứ tự ước số tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên tất cả các hệ số hóa | hàm mũ | Ngăn xếp O(log n) | Quá chậm | 
| Thừa số nguyên tố (chia thử) | O(\sqrt{n}) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng con số$n$và cố gắng trích xuất tất cả các yếu tố bắt đầu từ ước số nhỏ nhất có thể$2$. Chúng ta bắt đầu với các ước số nhỏ vì việc chia cho các số nhỏ hơn sẽ làm tăng tổng số các thừa số mạnh hơn so với việc sử dụng các phép chia tổng hợp lớn hơn. 
2. Với mỗi số nguyên$d$từ 2 đến$\sqrt{n}$, chia nhiều lần$n$qua$d$trong khi nó có thể chia được. Mỗi lần chia, chúng tôi ghi lại$d$như một yếu tố. Điều này đảm bảo rằng chúng tôi trích xuất đầy đủ tất cả các lần xuất hiện của$d$trước khi tiếp tục. 
3. Khi số chia không còn chia hết$n$, tăng$d$và tiếp tục. Điều này đảm bảo rằng mọi cấu trúc phức hợp cuối cùng được chia thành các thành phần nguyên tố vì bất kỳ thừa số tổng hợp nào cũng phải có ước số nguyên tố không vượt quá căn bậc hai của nó. 
4. Sau khi kết thúc vòng lặp, nếu giá trị còn lại của$n$lớn hơn 1 thì phải là số nguyên tố. Chúng tôi thêm nó làm yếu tố cuối cùng. 
5. Xuất ra tất cả các thừa số đã thu thập theo thứ tự, nối với nhau bằng ký tự 'x'. Thứ tự đương nhiên là không giảm vì chúng ta lặp các ước số theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Thuộc tính cốt lõi là bất kỳ hệ số tổng hợp nào cũng có thể được tinh chỉnh thành các thừa số nguyên tố mà không làm giảm số lượng số hạng và trên thực tế là tăng nó một cách nghiêm ngặt trừ khi thừa số đó đã là số nguyên tố. Bất kỳ yếu tố nào$a \times b$với$a, b > 1$đóng góp hai số hạng thay vì một, do đó việc phá vỡ các tổ hợp liên tục luôn cải thiện hoặc duy trì số lượng. Do đó, phép phân tách có độ dài tối đa chính xác là phép phân tích thành thừa số nguyên tố của$n$và không có cách nhóm thay thế nào có thể tạo ra nhiều nhân tử hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    res = []
    
    d = 2
    while d * d <= n:
        while n % d == 0:
            res.append(d)
            n //= d
        d += 1
    
    if n > 1:
        res.append(n)
    
    print("x".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình trích xuất nhân tố. Vòng lặp bên ngoài nâng cao các ước số có thể có, trong khi vòng lặp bên trong loại bỏ tất cả các lần xuất hiện của ước số hiện tại, đảm bảo tính bội số được nắm bắt hoàn toàn. Kiểm tra cuối cùng xử lý trường hợp số nguyên tố lớn vẫn còn sau phép chia, đây là tiêu chuẩn trong phép chia thử. 

Chi tiết triển khai chính đang sử dụng$d \times d \le n$là điều kiện dừng, đảm bảo tính đúng đắn ngay cả khi$n$co lại trong quá trình phân chia. Điều này tránh những lần lặp lại không cần thiết vượt quá giá trị đã giảm hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 12$Chúng tôi theo dõi cách các yếu tố được trích xuất. 

| Bước | d | n trước | Hành động | Yếu tố | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 12 | chia | 2 | 
| 2 | 2 | 6 | chia | 2, 2 | 
| 3 | 2 | 3 | dừng lại | 2, 2 | 
| 4 | 3 | 3 | chia | 2, 2, 3 | 

Đầu ra cuối cùng là`2x2x3`. 

Dấu vết này cho thấy sự phân chia lặp đi lặp lại sẽ phân hủy hoàn toàn các vật liệu tổng hợp một cách tự nhiên, tạo ra số lượng yếu tố tối đa. 

### Ví dụ 2:$n = 5$| Bước | d | n trước | Hành động | Yếu tố | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 5 | bỏ qua | | 
| 2 | 3 | 5 | bỏ qua | | 
| 3 | 4 | 5 | bỏ qua | | 
| 4 | cuối cùng | 5 | nối thêm | 5 | 

Đầu ra là`5`. 

Điều này thể hiện trường hợp nguyên tố trong đó không có sự phân tách nào làm tăng số lượng nhân tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(\sqrt{n}) | mỗi ước số được kiểm tra lên tới sqrt(n), với phép chia theo thời gian không đổi cho mỗi lần truy cập | 
| Không gian | O(log n) | lưu trữ thừa số nguyên tố của n | 

Các ràng buộc cho phép điều này một cách thoải mái, vì$\sqrt{10^5} \approx 316$, giúp giải pháp diễn ra cực kỳ nhanh chóng ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    def input():
        return sys.stdin.readline()
    
    n = int(input().strip())
    res = []
    d = 2
    while d * d <= n:
        while n % d == 0:
            res.append(d)
            n //= d
        d += 1
    if n > 1:
        res.append(n)
    return "x".join(map(str, res))

assert run("12\n") == "2x2x3"
assert run("5\n") == "5"
assert run("2\n") == "2"
assert run("16\n") == "2x2x2x2"
assert run("30\n") == "2x3x5"
assert run("49\n") == "7x7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 2 | kích thước đầu vào tối thiểu | 
| 16 | 2x2x2x2 | thừa số nguyên tố lặp lại | 
| 30 | 2x3x5 | thứ tự các số nguyên tố khác nhau | 
| 49 | 7x7 | hình vuông nguyên tố lớn lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào chính như$n = 13$, thuật toán thử các ước số 2 và 3, không tìm thấy giá trị nào và trực tiếp thêm 13 làm thừa số duy nhất. Dấu vết cho thấy không có sự phân chia nào xảy ra nên đầu ra vẫn là danh sách một phần tử. 

Đối với sức mạnh của hai như$n = 16$, phép chia lặp lại cho 2 tiếp tục cho đến khi giá trị trở thành 1, tạo ra bốn lần xuất hiện của 2. Mỗi phép chia sẽ tăng nghiêm ngặt số lượng hệ số, thể hiện trực tiếp điều kiện tối ưu. 

Đối với vật liệu tổng hợp hỗn hợp như$n = 30$, thuật toán trích xuất 2, rồi 3, rồi 5 theo thứ tự tăng dần. Vì mỗi số đều là số nguyên tố nên không thể phân tách thêm được nữa và bất kỳ nhóm thay thế nào như$6 \times 5$hoặc$3 \times 10$sẽ mang lại tổng các thừa số ít hơn, xác nhận tại sao việc phân tích đầy đủ là cần thiết.
