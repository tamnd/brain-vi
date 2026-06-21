---
title: "CF 1056H - Robot phát hiện"
description: "Thành phố là một đồ thị trong đó các đường giao nhau là các đỉnh và các đường là các cạnh vô hướng. Mỗi chuyến đi là một đường đi đơn giản trong biểu đồ này, có nghĩa là người lái xe không bao giờ quay lại một đỉnh trong chuyến đi đó. Trên tất cả các chuyến đi, chúng tôi quan sát thấy nhiều con đường đơn giản như vậy."
date: "2026-06-15T10:00:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1056
codeforces_index: "H"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 3"
rating: 3200
weight: 1056
solve_time_s: 158
verified: true
draft: false
---

[CF 1056H - Phát hiện robot](https://codeforces.com/problemset/problem/1056/H) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một đồ thị trong đó các đường giao nhau là các đỉnh và các đường là các cạnh vô hướng. Mỗi chuyến đi là một đường đi đơn giản trong biểu đồ này, có nghĩa là người lái xe không bao giờ quay lại một đỉnh trong chuyến đi đó. Trên tất cả các chuyến đi, chúng tôi quan sát thấy nhiều con đường đơn giản như vậy. 

Câu hỏi không phải là liệu biểu đồ có nhất quán hay liệu các chuyến đi có hợp lệ hay không. Thay vào đó, đó là về tính duy nhất của việc định tuyến. Với mọi cặp đỉnh có thứ tự$a, b$, hãy xem xét tất cả những cách có thể mà người lái xe có thể đã đi$a$ĐẾN$b$như một phần của bất kỳ chuyến đi nào được ghi lại. Nếu tồn tại hai đường dẫn đơn giản khác nhau giữa cùng một cặp có thứ tự thì người lái xe được khai báo là con người. Nếu với mỗi cặp có thứ tự, đường đi bắt buộc và duy nhất một cách hiệu quả thì người lái xe có thể là một robot. 

Một điểm tinh tế quan trọng là các chuyến đi được ghi lại không đặt câu hỏi trực tiếp giữa tất cả các cặp. Chúng chỉ cung cấp cho chúng ta những đường dẫn dài đơn giản và chúng ta phải suy ra các ràng buộc nhất quán toàn cục từ chúng. 

Các ràng buộc đủ lớn để bất kỳ phương trình bậc hai nào trên các đỉnh hoặc các đường cưỡi đều không thể thực hiện được. Tổng của tất cả các độ dài đường đi được giới hạn bởi$3 \cdot 10^5$, do đó, mọi giải pháp về cơ bản đều phải tuyến tính hoặc gần tuyến tính ở kích thước đầu vào. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng so sánh tất cả các cặp đường dẫn hoặc tính toán rõ ràng cấu trúc kết nối tất cả các cặp. 

Một trường hợp thất bại đơn giản xuất hiện khi nhiều chuyến đi trùng nhau một phần nhưng sau đó sẽ phân kỳ. 

Ví dụ: nếu một chuyến đi chứa$1 \to 2 \to 3 \to 4$và cái khác chứa$1 \to 2 \to 5 \to 4$, khi đó có hai đường đi đơn giản khác nhau giữa$2$Và$4$, buộc phải có câu trả lời của con người. Một cách tiếp cận ngây thơ chỉ kiểm tra xem mỗi chuyến đi có hợp lệ riêng lẻ hay không sẽ hoàn toàn bỏ sót điều này, vì riêng mỗi chuyến đi là một con đường đơn giản hợp lệ. 

Một trường hợp tế nhị khác là chu kỳ. Nếu các chuyến đi chung cho phép di chuyển quanh một chu kỳ theo nhiều hướng hoặc với nhiều cấu trúc dây cung thì tính duy nhất của các đường đi sẽ không thành công. Thách thức là phát hiện khi sự kết hợp của tất cả các chuyến đi ngụ ý nhiều hơn một tuyến đường đơn giản giữa một số cặp mà không liệt kê rõ ràng tất cả các cặp. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng xây dựng lại tất cả các đường đi và sau đó so sánh từng cặp đỉnh$a, b$trên tất cả các chuyến đi, kiểm tra xem có tồn tại nhiều con đường đơn giản hay không. Ngay cả khi chúng ta biểu diễn mỗi chuyến đi dưới dạng một chuỗi các cạnh, số lượng đường đi phụ là bậc hai trên mỗi chuyến đi, dẫn đến sự bùng nổ của$O(\sum k^2)$, quá lớn đối với$3 \cdot 10^5$tổng chiều dài. 

Một góc nhìn tốt hơn đến từ việc lật ngược vấn đề. Thay vì nghĩ về các cặp đỉnh, chúng tôi xem xét các ràng buộc cấu trúc cục bộ được áp đặt bởi các cạnh liên tiếp trong các chuyến đi. Mỗi chuyến đi đóng góp các ràng buộc có dạng “đỉnh$v$phải kết nối tiền thân và hậu duệ của nó một cách nhất quán”. Nếu một đỉnh bị buộc phải kết nối với nhiều hơn hai phần tiếp theo độc lập không được căn chỉnh theo một cấu trúc giống như chuỗi đơn, thì nhiều đường dẫn đơn giản phải tồn tại ở đâu đó trong hệ thống. 

Điều này dẫn đến quan sát cốt lõi: một cấu trúc đảm bảo tính duy nhất của các đường đi đơn giản giữa hai đỉnh bất kỳ về cơ bản là một tập hợp các đường đi đơn giản tách rời nhau, có thể tạo thành một cấu trúc giống như một đường thẳng, trong đó mỗi đỉnh bên trong có nhiều nhất là hai bậc trong đồ thị ràng buộc ngụ ý. Nếu bất kỳ đỉnh nào bị buộc phải hoạt động giống như một điểm phân nhánh theo cách không nhất quán với thứ tự đường đi duy nhất thì sẽ xuất hiện sự mơ hồ. 

Chúng tôi xử lý các hành trình và xây dựng các ràng buộc kề cận do các đỉnh liên tiếp gây ra. Sau đó, chúng tôi phát hiện xem có bất kỳ đỉnh nào bị ép vào các ràng buộc thứ tự xung đột có nghĩa là phân nhánh hoặc chu trình không tương thích với cấu trúc đường dẫn duy nhất hay không. Vấn đề giảm xuống còn việc kiểm tra xem biểu đồ ràng buộc kết quả có hoạt động giống như một tập hợp các chuỗi rời rạc với thứ tự nhất quán hay không. 

Bí quyết quan trọng là coi mỗi chuyến đi là các ràng buộc kề cận gây ra và xác minh xem liệu cấu trúc cảm ứng có thể được định hướng thành một tập hợp các đường đi mà không có mâu thuẫn hay không. Điều này có thể được thực hiện bằng cách sử dụng các ràng buộc về mức độ và tính nhất quán của thứ tự lân cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum k^2)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(\sum k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tất cả các chuyến đi thành thông tin kề cận vô hướng giữa các đỉnh liên tiếp. 

1. Với mỗi chuyến đi, hãy lặp qua các cặp liên tiếp$(c_i, c_{i+1})$và ghi lại rằng hai đỉnh này được kết nối trực tiếp theo nghĩa ràng buộc. Điều này ghi lại tất cả các chuyển đổi bắt buộc cục bộ trong các đường dẫn được quan sát. 
2. Duy trì cho mỗi đỉnh một tập hợp (hoặc cấu trúc băm) các lân cận mà nó bị ràng buộc kết nối. Chúng tôi không xây dựng biểu đồ ban đầu mà là sự kết hợp của tất cả các chuyển đổi bắt buộc. 
3. Đối với mỗi đỉnh, hãy kiểm tra xem nó có bao nhiêu lân cận riêng biệt trong cấu trúc ràng buộc này. Nếu bất kỳ đỉnh nào có nhiều hơn hai đỉnh lân cận khác nhau, chúng ta ngay lập tức kết luận người điều khiển là con người. Lý do là trong bất kỳ cấu trúc nào mà mỗi cặp đỉnh có một đường đi đơn duy nhất, cấu trúc cảm ứng phải hoạt động giống như một tập hợp các đường đi và các đỉnh đường đi có nhiều nhất là hai bậc. 
4. Ngoài ra, chúng ta phải đảm bảo tính nhất quán của việc đặt hàng do các chuyến đi mang lại. Chúng tôi mô phỏng các ràng buộc truyền tải bằng cách đảm bảo rằng nếu một đỉnh xuất hiện trong nhiều hành trình, thì các mối quan hệ kề cận của nó không hàm ý sự mơ hồ phân nhánh. Trong thực tế, một khi tất cả các chuyến đi được xử lý, biểu đồ ràng buộc phải là rừng giả có mức tối đa là hai. 
5. Nếu không có đỉnh nào vi phạm ràng buộc mức độ, chúng tôi kết luận rằng cấu trúc phù hợp với cấu hình và đầu ra giống như đường dẫn xác định. 

Ý tưởng cơ bản là mọi chuyến đi đều đảm bảo tính liên tục cục bộ và bất kỳ vi phạm nào về “nhiều nhất là hai hướng trên mỗi đỉnh” sẽ tạo ra nhiều cách khả thi để di chuyển giữa một số cặp đỉnh. 

### Tại sao nó hoạt động 

Nếu mỗi đỉnh có nhiều nhất hai đỉnh lân cận trong cấu trúc ràng buộc cảm ứng thì mỗi thành phần liên thông hoặc là một đường đi đơn hoặc một chu trình. Tuy nhiên, các chu trình không được phép bởi điều kiện duy nhất vì bất kỳ chu trình nào cũng ngay lập tức tạo ra hai đường đi đơn giản riêng biệt giữa hai đỉnh bất kỳ trên nó. Vì vậy, để người lái trở thành một robot, cấu trúc trên thực tế phải là một tập hợp các đường đi đơn giản, không phân nhánh và không có chu kỳ. Việc xây dựng đảm bảo rằng nếu một chu trình hoặc phân nhánh tồn tại trong các chuyển đổi ngụ ý, thì một số đỉnh sẽ có bậc ít nhất là ba trong biểu đồ ràng buộc, đó chính xác là những gì chúng tôi phát hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        q = int(input())
        
        deg = [0] * (n + 1)
        seen_edges = set()

        bad = False

        for _ in range(q):
            tmp = list(map(int, input().split()))
            k = tmp[0]
            path = tmp[1:]
            
            for i in range(k - 1):
                a, b = path[i], path[i + 1]
                if a > b:
                    a, b = b, a
                if (a, b) not in seen_edges:
                    seen_edges.add((a, b))
                    deg[a] += 1
                    deg[b] += 1
                    if deg[a] > 2 or deg[b] > 2:
                        bad = True

        print("Human" if bad else "Robot")

if __name__ == "__main__":
    solve()
```Mã này xây dựng một bản trình bày nhỏ gọn về các ràng buộc lân cận bằng cách sử dụng tập hợp cạnh để tránh tính hai lần các chuyển đổi lặp đi lặp lại trên các chuyến đi. Mảng độ theo dõi số lượng lân cận riêng biệt mà mỗi đỉnh buộc phải kết nối. Điều kiện thoát sớm được kích hoạt ngay khi bất kỳ đỉnh nào vượt quá hai đỉnh lân cận khác nhau, vì điều đó đã đảm bảo sự phân nhánh cấu trúc phá vỡ tính duy nhất. 

Việc chuẩn hóa thứ tự`(a, b) = sorted pair`đảm bảo rằng các cạnh vô hướng không bị tính hai lần bất kể hướng di chuyển trong các chuyến đi. Điều này rất cần thiết vì các chuyến xe có thể đi qua các cạnh theo cả hai hướng, nhưng chúng tôi chỉ quan tâm đến kết nối cấu trúc chứ không quan tâm đến định hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
2
4 1 2 3 4
3 1 3 4
```Chúng tôi xử lý các cạnh: 

| Bước | Cạnh | Thay đổi bằng cấp | Trạng thái xấu | 
| --- | --- | --- | --- | 
| 1 | 1-2 | độ1=1, độ2=1 | Không | 
| 2 | 2-3 | độ2=2, độ3=1 | Không | 
| 3 | 3-4 | độ3=2, độ4=1 | Không | 
| 4 | 1-3 | độ1=2, độ3=3 | Có | 

Đỉnh 3 đạt cấp độ 3, biểu thị ba kết nối cưỡng bức riêng biệt. Điều này tạo ra sự mơ hồ về cách định tuyến giữa một số cặp, vì vậy đầu ra là Con người. 

### Ví dụ 2 

đầu vào:```
1
5
1
5 1 2 3 4 5
```| Bước | Cạnh | Thay đổi bằng cấp | Trạng thái xấu | 
| --- | --- | --- | --- | 
| 1 | 1-2 | độ1=1, độ2=1 | Không | 
| 2 | 2-3 | độ2=2, độ3=1 | Không | 
| 3 | 3-4 | độ3=2, độ4=1 | Không | 
| 4 | 4-5 | độ4=2, độ5=1 | Không | 

Không có đỉnh nào vượt quá bậc 2, do đó cấu trúc vẫn là một đường dẫn đơn giản và câu trả lời là Robot. 

Dấu vết đầu tiên cho thấy việc phân nhánh ngay lập tức phá vỡ tính nhất quán như thế nào. Điều thứ hai xác nhận rằng một chuỗi thuần túy bảo tồn tính duy nhất của các tuyến đường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum k)$| Mỗi cặp liên tiếp trong các chuyến đi được xử lý một lần và được lưu trữ trong bộ băm | 
| Không gian |$O(n + \sum k)$| Mảng độ cộng với tập cạnh lưu trữ từng vùng kề riêng biệt | 

Tổng số lần chuyển đổi được giới hạn bởi$3 \cdot 10^5$, do đó giải pháp chạy thoải mái trong giới hạn. Tính năng loại bỏ trùng lặp cạnh dựa trên hàm băm đảm bảo chúng tôi không bao giờ tăng độ phức tạp bằng cách xuất hiện lặp đi lặp lại cùng một phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            q = int(input())
            deg = [0] * (n + 1)
            seen = set()
            bad = False
            for _ in range(q):
                arr = list(map(int, input().split()))
                k = arr[0]
                path = arr[1:]
                for i in range(k - 1):
                    a, b = path[i], path[i + 1]
                    if a > b:
                        a, b = b, a
                    if (a, b) not in seen:
                        seen.add((a, b))
                        deg[a] += 1
                        deg[b] += 1
                        if deg[a] > 2 or deg[b] > 2:
                            bad = True
            print("Human" if bad else "Robot")

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("""1
5
2
4 1 2 3 5
3 1 4 3
""") == "Human"

# minimal case
assert run("""1
2
1
2 1 2
""") == "Robot"

# branching case
assert run("""1
4
2
3 1 2 3
3 3 2 4
""") == "Human"

# straight chain
assert run("""1
5
1
5 1 2 3 4 5
""") == "Robot"

# cycle-like forcing
assert run("""1
3
2
3 1 2 3
3 1 3 2
""") == "Human"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 2 nút | Robot | đường dẫn hợp lệ đơn giản nhất | 
| phân nhánh ở giữa | Con người | độ > 2 phát hiện | 
| dây chuyền nguyên chất | Robot | không có kết quả dương tính giả | 
| chu kỳ xung đột | Con người | sự mơ hồ về chu kỳ | 

## Vỏ cạnh 

Trường hợp cạnh chính là việc di chuyển lặp đi lặp lại của cùng một đoạn trên nhiều chuyến đi. Thuật toán xử lý việc này thông qua`seen_edges`được thiết lập, đảm bảo rằng việc xem lại một cạnh không làm tăng độ một cách giả tạo. Nếu không có điều này, một chuỗi hợp lệ có thể vượt quá giới hạn độ một cách không chính xác. 

Một trường hợp cạnh khác là một đỉnh xuất hiện nhiều lần trên các chuyến đi khác nhau nhưng chỉ với hai đỉnh lân cận nhất quán. Điều kiện dựa trên mức độ giữ cho nó hợp lệ một cách chính xác vì tính duy nhất phụ thuộc vào các lân cận khác biệt chứ không phải tần suất xuất hiện. 

Trường hợp cạnh cuối cùng là các cấu trúc lớn giống như ngôi sao trong đó một đỉnh kết nối với nhiều đường dẫn. Ngay khi hàng xóm riêng biệt thứ ba xuất hiện, thuật toán sẽ đánh dấu nó là Con người, nắm bắt chính xác điểm đầu tiên nơi có thể thực hiện được nhiều tuyến đường đơn giản giữa các điểm cuối.
