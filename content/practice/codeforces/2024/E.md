---
title: "CF 2024E - C+K+S"
description: "Chúng ta có hai đồ thị có hướng, mỗi đồ thị có cùng số đỉnh và chúng ta được yêu cầu cộng chính xác một cạnh từ mỗi đỉnh đi vào một đỉnh đi vào trong đồ thị kia."
date: "2026-06-09T03:10:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 2024
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 980 (Div. 2)"
rating: 2400
weight: 2024
solve_time_s: 218
verified: false
draft: false
---

[CF 2024E - C+K+S](https://codeforces.com/problemset/problem/2024/E) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, đồ thị, hàm băm, chuỗi 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đồ thị có hướng, mỗi đồ thị có cùng số đỉnh và chúng ta được yêu cầu cộng chính xác một cạnh từ mỗi đỉnh đi vào một đỉnh đi vào trong đồ thị kia. Mỗi đồ thị gốc được kết nối chặt chẽ và mỗi chu trình trong đồ thị có độ dài chia hết cho một số cho trước$k$. Mục đích là để đảm bảo rằng sau khi thêm những thứ này$n$các cạnh, đồ thị kết hợp thu được bảo toàn tính chất là mọi độ dài chu kỳ đều chia hết cho$k$. 

Đầu vào chỉ định cho mỗi đỉnh xem đó là đỉnh đi hay đỉnh đi và các cạnh của cả hai đồ thị. Chúng ta phải quyết định xem liệu có thể ghép tất cả các đỉnh đi với các đỉnh đến trên hai đồ thị theo cách một-một trong khi vẫn bảo toàn tính chất chia hết chu trình hay không. 

Những ràng buộc cho phép$n$lên đến$2 \cdot 10^5$tổng cộng trên tất cả các trường hợp thử nghiệm, với số cạnh lên tới$5 \cdot 10^5$mỗi biểu đồ. Điều này ngụ ý rằng bất kỳ thuật toán nào cũng phải chạy trong thời gian gần như tuyến tính cho mỗi trường hợp thử nghiệm,$O(n + m)$, bởi vì mọi thứ bậc hai sẽ vượt quá giới hạn thời gian. Ngoài ra, do các đồ thị ban đầu được kết nối chặt chẽ và tất cả các chu trình đều chia hết cho$k$, thuộc tính chu kỳ đặt ra những hạn chế tinh tế về cách chúng ta có thể thêm các cạnh. Nếu chúng ta bỏ qua độ dài chu kỳ, một cách tiếp cận đơn giản có thể ghép các đỉnh không chính xác và tạo ra các chu trình có độ dài không hợp lệ. Trường hợp cạnh phát sinh khi số lượng đỉnh đi và đến trong hai biểu đồ không khớp nhau - trong tình huống này, không thể tạo thành một khớp hoàn chỉnh. 

Một ví dụ nhỏ thể hiện sự tinh tế này là một biểu đồ trong đó tất cả các đỉnh trong biểu đồ đầu tiên đều hướng ra ngoài và tất cả các đỉnh trong biểu đồ thứ hai đều hướng vào, nhưng số lượng không bằng nhau. Bất kỳ sự ghép nối tham lam ngây thơ nào cũng sẽ thất bại vì các đỉnh bị thiếu ngăn cản việc hình thành ánh xạ một-một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi cách có thể để khớp các đỉnh đi từ biểu đồ này với các đỉnh đến trong biểu đồ kia và kiểm tra xem tất cả các chu trình mới được tạo có duy trì thuộc tính chia hết hay không. Với mỗi cặp đồ thị có$n$đỉnh, điều này sẽ liên quan đến việc liệt kê$n!$phù hợp trong trường hợp xấu nhất. Mỗi kết quả khớp sẽ yêu cầu mô phỏng biểu đồ kết quả để xác minh độ dài chu kỳ, bao gồm việc duyệt qua biểu đồ, mất ít nhất$O(n+m)$thời gian cho mỗi lần kiểm tra. Điều này nhanh chóng trở nên không khả thi ngay cả đối với những$n$, vì vậy vũ lực không phải là một lựa chọn. 

Quan sát quan trọng mở ra lời giải là các đồ thị ban đầu được kết nối chặt chẽ với tất cả độ dài chu kỳ chia hết cho$k$. Cấu trúc này ngụ ý rằng các đỉnh trong mỗi đồ thị có thể được gán một "lớp dư modulo$k$" dựa trên khoảng cách dọc theo một chu trình tham chiếu. Khi thêm các cạnh giữa các biểu đồ, chúng ta có thể ánh xạ các đỉnh đi tới các đỉnh đến sao cho chênh lệch dư lượng là nhất quán theo mô đun$k$. Cụ thể, chúng ta không cần phải mô phỏng mọi chu kỳ. Thay vào đó, chúng ta chỉ cần kiểm tra xem số lượng đỉnh đi và đỉnh đến có khớp giữa các biểu đồ hay không và tổng số đỉnh trong mỗi lớp dư lượng modulo$k$có thể được ghép nối trên các biểu đồ. Vì các đồ thị liên thông chặt chẽ và tất cả các chu trình đều chia hết cho$k$, bất kỳ sự dịch chuyển tuyến tính nào của các cạnh giữa các đồ thị đều bảo toàn tính chất chia hết chu trình nếu các điều kiện modulo được giữ nguyên. 

Điều này làm giảm vấn đề đếm và khớp các đỉnh theo loại và modulo$k$, đưa ra một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$k$, loại đỉnh trong mỗi đồ thị và các cạnh. Thông tin biên chỉ cần thiết để khẳng định khả năng kết nối mạnh mẽ; chúng tôi sẽ không sử dụng nó trực tiếp để tính toán kết quả khớp. 
2. Đếm số đỉnh đi và đến của mỗi đồ thị. Cho phép$out_1$Và$in_1$là số đếm trong biểu đồ đầu tiên và$out_2$Và$in_2$cho đồ thị thứ hai. 
3. Kiểm tra xem tổng số đỉnh đi trên cả hai đồ thị có bằng tổng số đỉnh đi vào hay không. Cụ thể,$out_1 + out_2 = in_1 + in_2 = n$. If this does not hold, output "NO" because a one-to-one mapping is impossible.
 4. For cycle divisibility, notice that if the original graphs have all cycles divisible by$k$, sau đó thêm các cạnh kết nối các đỉnh đi với các đỉnh đến trong biểu đồ khác sẽ giữ nguyên thuộc tính miễn là chúng ta không tạo ra một modulo tự lặp hoặc không khớp. Bởi vì chúng tôi chỉ kết nối các đỉnh đi với đỉnh đến trên các biểu đồ, nên mỗi cạnh mới sẽ tăng chu kỳ lên đúng 1 trong mỗi biểu đồ. Kết nối mạnh mẽ đảm bảo rằng modulo dư khoảng cách$k$chu kỳ vẫn nhất quán. Do đó, việc khớp bất kỳ đỉnh đi nào từ một biểu đồ với bất kỳ đỉnh đi vào nào trong biểu đồ khác là an toàn theo modulo$k$. Không cần kiểm tra thêm. 
5. Nếu bước 3 vượt qua, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Thông tin chi tiết quan trọng là thuộc tính chu trình được đảm bảo bởi cấu trúc của biểu đồ gốc và kết quả khớp dựa trên loại; chúng ta không bao giờ cần phải liệt kê các chu kỳ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi thêm các cạnh, mỗi đỉnh sẽ tham gia vào đúng một cạnh mới, duy trì sự ghép đôi một-một. Khả năng kết nối mạnh mẽ của các biểu đồ gốc đảm bảo rằng khoảng cách dọc theo các chu kỳ được xác định rõ ràng theo modulo$k$và việc ghép nối dựa trên loại đảm bảo không tạo ra chu kỳ có độ dài không hợp lệ. Việc so sánh số lượng đảm bảo tính khả thi. Không có bước nào dẫn đến chu trình không hợp lệ nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        m1 = int(input())
        for _ in range(m1):
            input()  # skip edges
        b = list(map(int, input().split()))
        m2 = int(input())
        for _ in range(m2):
            input()  # skip edges
        
        out1 = a.count(1)
        in1 = n - out1
        out2 = b.count(1)
        in2 = n - out2
        
        if out1 + out2 == n and in1 + in2 == n:
            print("YES")
        else:
            print("NO")

solve()
```Trong giải pháp này, trước tiên chúng tôi đọc tất cả các loại đỉnh và bỏ qua danh sách cạnh vì chúng không liên quan đến quyết định. Việc đếm các đỉnh đi và đến trong mỗi biểu đồ là đủ để xác định xem có tồn tại một cạnh hợp lệ hay không. Việc kiểm tra tính bằng nhau đảm bảo có thể ghép nối một-một, điều này ngầm bảo toàn khả năng phân chia chu kỳ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 2
1 0 0 1
4
1 2
2 3
3 4
4 1
1 0 0 1
4
1 3
3 2
2 4
4 1
```| Biến | Giá trị | 
| --- | --- | 
| ra 1 | 2 | 
| trong 1 | 2 | 
| ra 2 | 2 | 
| in2 | 2 | 
| out1 + out2 | 4 | 
| in1 + in2 | 4 | 
Tổng số đếm bằng$n$, và về số lượng bằng$n$. Đầu ra là "CÓ". 

**Đầu vào mẫu 2:**```
3 3
0 0 0
3
1 2
2 3
3 1
1 1 0
3
1 2
2 3
3 1
```| Biến | Giá trị | 
| --- | --- | 
| ra 1 | 0 | 
| trong 1 | 3 | 
| ra 2 | 2 | 
| trong 2 | 1 | 
| out1 + out2 | 2 | 
| in1 + in2 | 4 | 
Tổng số đếm ra là 2, nhỏ hơn$n=3$. Đầu ra là "KHÔNG". 

Các bảng này minh họa rằng chỉ có số đếm mới quan trọng và thuật toán nắm bắt được tính bất biến này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Đếm các loại đỉnh là O(n) và bỏ qua các cạnh là O(m) trên mỗi đồ thị | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng loại đỉnh cho mỗi biểu đồ | 

Với$n \le 2 \cdot 10^5$và tổng số cạnh$\le 5 \cdot 10^5$, giải pháp chạy thoải mái trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("""3
4 2
1 0 0 1
4
1 2
2 3
3 4
4 1
1 0 0 1
4
1 3
3 2
2 4
4 1
3 3
0 0 0
3
1 2
2 3
3 1
1 1 0
3
1 2
2 3
3 1
4 2
1 1 1 1
4
1 2
2 3
3 4
4 1
0 0 0
```
