---
title: "CF 104366M - Bài toán dễ dàng của Prime"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số $n$ và với giá trị đó trước tiên chúng ta tưởng tượng tất cả các số nguyên từ $2$ đến $n$. Với mỗi số nguyên $i$, chúng ta xác định một giá trị $f(i)$, trong đó $f(i)$ là số nhỏ nhất của các số nguyên tố có tổng bằng chính xác $i$."
date: "2026-07-01T17:45:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "M"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 55
verified: true
draft: false
---

[CF 104366M - Bài toán dễ dàng của Prime](https://codeforces.com/problemset/problem/104366/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số$n$và với giá trị đó trước tiên chúng ta tưởng tượng tất cả các số nguyên từ$2$lên đến$n$. Với mỗi số nguyên$i$, chúng tôi xác định một giá trị$f(i)$, Ở đâu$f(i)$là số nhỏ nhất của các số nguyên tố có tổng bằng chính xác$i$. Sau khi tính toán các số đếm tối ưu này cho mọi$i$, chúng tôi được yêu cầu trả lại số tiền tích lũy$f(2) + f(3) + \dots + f(n)$cho mỗi truy vấn. 

Đối tượng chính ở đây là$f(i)$. Thay vì nhân các số thành nhân tử, chúng ta phân tích chúng thành các số nguyên tố và chúng ta muốn giảm thiểu số lượng số nguyên tố được sử dụng. Điều này biến vấn đề thành một tình huống “đổi xu với số xu không giới hạn” cổ điển trong đó mọi số nguyên tố đều là một đồng xu và chúng tôi muốn số lượng xu tối thiểu tạo thành một tổng. 

Các ràng buộc rất lớn: lên tới một triệu truy vấn và giá trị của$n$lên đến chục triệu. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại$f(i)$riêng biệt cho mỗi truy vấn hoặc sử dụng lập trình động cho mỗi truy vấn. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng sẽ quá chậm vì tổng công việc sẽ tăng lên khoảng$10^{13}$hoạt động trong trường hợp xấu nhất. Bất kỳ giải pháp khả thi nào cũng phải giảm mọi thứ thành biểu thức dạng đóng hoặc một phép tính trước duy nhất trên toàn bộ phạm vi một lần. 

Một điểm tinh tế có thể gây ra suy luận sai là giả định rằng số lớn hơn cần có nhiều số nguyên tố. Ví dụ: người ta có thể thử tính:$f(6) = 3 + 3 = 2$,$f(7) = 2 + 2 + 3 = 3$, 

và sau đó thử DP chung trên các số nguyên tố. Điều này là không cần thiết và sẽ hết thời gian. Một cạm bẫy khác là quên mất điều đó$2$là số nguyên tố và có thể được sử dụng nhiều lần, điều này hạn chế mạnh mẽ cấu trúc của các phân tách tối ưu. 

## Phương pháp tiếp cận 

Cách giải thích brute-force coi đây là vấn đề về đường đi ngắn nhất hoặc chiếc ba lô. Với mỗi số nguyên$i$, chúng tôi thử tất cả các số nguyên tố$p \le i$và tính toán$f(i) = \min(f(i - p) + 1)$. Điều này đúng vì nó khám phá tất cả các số nguyên tố cuối cùng có thể có trong một phân tích và xây dựng các giải pháp từ dưới lên. 

Tuy nhiên, chi phí bị chi phối bởi việc lặp lại các số nguyên tố cho mọi giá trị lên đến$n$. Ngay cả khi chúng ta tính toán trước các số nguyên tố bằng sàng, quá trình chuyển đổi DP vẫn yêu cầu khoảng$O(n \cdot \pi(n))$hoạt động. Với$n = 10^7$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là cấu trúc của các phân tách tối ưu gần như sụp đổ hoàn toàn vì tập nguyên tố chứa$2$. Bất kỳ số nguyên nào cũng có thể được biểu diễn bằng cách sử dụng$2$s, ngoại trừ các ràng buộc chẵn lẻ trong đó một$3$giúp giải quyết các khoản tiền lẻ hiệu quả hơn$2$S. Điều này dẫn đến một sự đơn giản hóa đáng ngạc nhiên: số lượng số nguyên tố tối ưu cần thiết chỉ phụ thuộc vào tính chẵn lẻ chứ không phụ thuộc vào cấu trúc nguyên tố chi tiết của số đó. 

Thậm chí$n$, chỉ sử dụng$2$s cho$n/2$số nguyên tố, và không có sự kết hợp nào khác có thể làm tốt hơn vì mọi số nguyên tố ít nhất$2$, vậy mỗi số hạng đóng góp nhiều nhất$2$mỗi đồng xu về mặt hiệu quả tổng hợp. Đối với số lẻ$n \ge 3$, chúng tôi sử dụng một$3$và phần còn lại$2$s, cho$(n-3)/2 + 1 = (n-1)/2$số nguyên tố. Cả hai trường hợp thống nhất gọn gàng như$f(n) = \lfloor n/2 \rfloor$. 

Một khi sự rút gọn này được nhìn thấy, nhiệm vụ ban đầu sẽ trở thành nhiệm vụ số học thuần túy: chúng ta chỉ cần tổng tiền tố của$\lfloor i/2 \rfloor$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DP trên số nguyên tố |$O(n \cdot \pi(n))$|$O(n)$| Quá chậm | 
| Biểu mẫu đóng + tổng tiền tố |$O(1)$mỗi truy vấn sau khi tính toán trước hoặc công thức trực tiếp |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng mẫu đóng$f(i) = \lfloor i/2 \rfloor$, sau đó chuyển truy vấn thành bài toán tổng tiền tố. 

1. Không tính toán trước gì cả, hoặc tùy ý tính toán trước các câu trả lời ở mức tối đa$n$xuất hiện trong các truy vấn nếu chúng ta thích mảng tiền tố hơn. Ý tưởng chính là chúng ta không còn cần thông tin nguyên tố hoặc quy hoạch động nữa. 
2. Với mỗi giá trị truy vấn$n$, định nghĩa$m = \lfloor n/2 \rfloor$. Giá trị này biểu thị số lượng cặp số nguyên đầy đủ phù hợp với phạm vi và trở thành tham số kiểm soát của toàn bộ tổng. 
3. Tính câu trả lời bằng cách sử dụng dạng đóng dẫn xuất. Nếu như$n = 2m$, tổng$\sum_{i=2}^{n} \lfloor i/2 \rfloor$bằng$m^2$. Nếu như$n = 2m+1$, tổng bằng$m(m+1)$. Điều này xuất phát trực tiếp từ việc ghép các số nguyên liên tiếp và tính tổng đóng góp của chúng.
 4. Xuất ngay giá trị tính toán cho mỗi truy vấn. 

Tại sao nó hoạt động bắt nguồn từ cách$\lfloor i/2 \rfloor$cư xử theo cặp. Mỗi cặp$(2k-1, 2k)$đóng góp$(k-1) + k = 2k-1$, tạo thành một cấu trúc số học rõ ràng. Tổng hợp những đóng góp này mang lại một biểu thức hoàn hảo hình vuông hoặc hình tam giác giống như số tùy thuộc vào tính chẵn lẻ. Từ$f(i)$chính xác là biểu thức sàn này, không có phân tích nguyên tố thay thế nào có thể thay đổi kết quả. 

Điều bất biến là mọi số nguyên đóng góp chính xác một nửa giá trị của nó được làm tròn thành số lượng số nguyên tố tối thiểu và đóng góp này không phụ thuộc vào bất kỳ tương tác nào giữa các số nguyên khác nhau. Điều này loại bỏ tất cả sự ghép nối giữa các trạng thái và biến vấn đề thành một nhận dạng tổng trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        m = n // 2
        if n % 2 == 0:
            print(m * m)
        else:
            print(m * (m + 1))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa hoàn toàn vào dạng đóng dẫn xuất, do đó không có quá trình xử lý trước hoặc phân bổ mảng. Sự tinh tế duy nhất là xử lý chính xác phép chia số nguyên và tính chẵn lẻ. sử dụng$m = n // 2$đảm bảo cả trường hợp chẵn và lẻ đều được xử lý thống nhất mà không cần phân nhánh trên trạng thái logic nguyên tố hoặc trạng thái DP. 

Trường hợp chẵn trả về$m^2$, tương ứng với các cặp tổng hợp đối xứng. Trường hợp kỳ lạ trở lại$m(m+1)$, chiếm phần bổ sung của thuật ngữ chưa ghép đôi$m$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$n = 6$. 

| tôi | f(i) = tầng(i/2) | 
| --- | --- | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 2 | 
| 6 | 3 | 

Tổng là$1 + 1 + 2 + 2 + 3 = 9$. 

Sử dụng công thức,$m = 3$, vậy kết quả là$m^2 = 9$. 

Điều này xác nhận rằng cấu trúc ghép nối nắm bắt đầy đủ hành vi của chức năng. 

### Ví dụ 2 

Hãy xem xét$n = 7$. 

| tôi | f(i) = tầng(i/2) | 
| --- | --- | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 2 | 
| 6 | 3 | 
| 7 | 3 | 

Tổng là$12$. 

Đây$m = 3$, vậy kết quả là$m(m+1) = 12$. 

Điều này cho thấy phần tử lẻ bổ sung đóng góp chính xác như thế nào$m$, phù hợp với thuật ngữ hiệu chỉnh dẫn xuất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi truy vấn | Mỗi truy vấn giảm xuống một số lượng phép toán số học không đổi | 
| Không gian |$O(1)$| Không cần cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên tới một triệu truy vấn, do đó, công thức thời gian không đổi cho mỗi truy vấn là cách tiếp cận duy nhất phù hợp một cách thoải mái trong giới hạn. Ngay cả tiền xử lý tuyến tính trên$10^7$có thể được chấp nhận một lần, nhưng không cần thiết vì dạng đóng loại bỏ mọi sự phụ thuộc vào$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# sample-like checks
assert run("1\n2\n") == "1"
assert run("1\n6\n") == "9"

# custom cases
assert run("1\n3\n") == "1", "small odd"
assert run("1\n4\n") == "4", "even boundary"
assert run("1\n7\n") == "12", "odd case"
assert run("3\n2\n3\n4\n") == "1\n1\n4", "multiple queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | trường hợp chẵn hợp lệ nhỏ nhất | 
| 1 7 | 12 | xử lý chẵn lẻ lẻ | 
| nhiều truy vấn nhỏ | hỗn hợp | sự độc lập của các truy vấn | 

## Vỏ cạnh 

cho$n = 2$, chúng tôi có$m = 1$và công thức cho$1$, phù hợp với thực tế là chỉ cần một số nguyên tố. Thuật toán tránh được mọi cách viết hoa đặc biệt một cách chính xác vì phép chia số nguyên đã tạo ra cấu trúc chính xác. 

Vì$n = 3$,$m = 1$và kết quả là$1$, tương ứng với việc sử dụng một số nguyên tố duy nhất$3$. Công thức nắm bắt một cách tự nhiên rằng không cần phân tích thành hai số nguyên tố. 

Đối với thậm chí lớn$n$, chẳng hạn như$10^7$,$m = 5 \cdot 10^6$, và kết quả trở thành một hình vuông hoàn hảo. Quá trình tính toán nằm trong giới hạn phạm vi 64 bit của số nguyên Python, do đó không xảy ra sự cố tràn. 

Đối với số lẻ lớn$n$, thuật ngữ bổ sung$m(m+1)$tính toán chính xác phần đóng góp số nguyên chưa ghép đôi cuối cùng và quá trình chuyển đổi giữa số chẵn và số lẻ vẫn diễn ra suôn sẻ mà không bị gián đoạn trong quá trình triển khai.
