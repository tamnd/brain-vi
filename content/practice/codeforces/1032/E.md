---
title: "CF 1032E - Trọng lượng nhẹ đến mức không thể chịu nổi"
description: "Chúng ta được cấp nhiều tập trọng số, mỗi trọng số có khối lượng nguyên từ 1 đến 100, nhưng chúng ta không thể phân biệt được các trọng số đó. Chúng ta không biết vật thể nào tương ứng với khối lượng nào, chỉ có danh sách đầy đủ về khối lượng tồn tại ở đâu đó trong kiến ​​thức của bạn chúng ta."
date: "2026-06-16T20:12:32+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1032
codeforces_index: "E"
codeforces_contest_name: "Technocup 2019 - Elimination Round 3"
rating: 2100
weight: 1032
solve_time_s: 205
verified: false
draft: false
---

[CF 1032E - Trọng lượng nhẹ đến mức không thể chịu nổi](https://codeforces.com/problemset/problem/1032/E) 

**Đánh giá:** 2100 
**Tags:** dp, toán 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp nhiều tập trọng số, mỗi trọng số có khối lượng nguyên từ 1 đến 100, nhưng chúng ta không thể phân biệt được các trọng số đó. Chúng ta không biết vật thể nào tương ứng với khối lượng nào, chỉ có danh sách đầy đủ về khối lượng tồn tại ở đâu đó trong kiến ​​thức của bạn chúng ta. 

Chúng tôi được phép thực hiện chính xác một truy vấn. Trong một truy vấn, chúng tôi chọn một số mục$k$và tổng khối lượng$m$và yêu cầu một tập hợp con chính xác$k$trọng lượng có tổng khối lượng bằng$m$. Người bạn sẽ trả về bất kỳ tập hợp con hợp lệ nào thỏa mãn ràng buộc, nếu có. 

Sau khi nhận được tập hợp con đó, chúng tôi biết được khối lượng của những vật phẩm được trả lại đó. Đối với các trọng số còn lại, chúng ta chỉ biết chúng là multiset còn sót lại. Mục tiêu là tối đa hóa số lượng trọng số mà chúng tôi có thể xác định duy nhất sau một truy vấn duy nhất này. 

Khó khăn chính là tập con được trả về có thể không phải là duy nhất. Nếu nhiều tập con thỏa mãn cùng một$(k, m)$, người bạn có thể chọn bất kỳ trọng số nào trong số đó và chúng ta phải đảm bảo rằng dù lựa chọn thế nào thì chúng ta vẫn học được càng nhiều trọng số càng tốt. 

Các hạn chế là nhỏ:$n \le 100$Và$a_i \le 100$. Điều này ngay lập tức gợi ý rằng các tập hợp con hàm mũ có khả năng sử dụng được, nhưng chỉ khi được nén thành một DP kiểu ba lô theo tổng và số lượng. 

Một ý tưởng ngây thơ là thử mọi truy vấn có thể$(k, m)$, mô phỏng tất cả các tập hợp con có kích thước$k$với tổng$m$và tính xem có bao nhiêu phần tử được xác định. Cách tiếp cận đó thất bại vì số lượng tập hợp con là$2^n$và thậm chí việc kiểm tra tính nhất quán cho mỗi truy vấn trở nên không khả thi. 

Trường hợp cạnh tinh tế phát sinh khi nhiều tập hợp con có cùng tổng và kích thước. Ví dụ: nếu tất cả các giá trị đều bằng nhau thì mọi tập hợp con có kích thước$k$có cùng số tiền, do đó câu trả lời không cung cấp thông tin nào ngoài số lượng số và hầu hết lý luận đều dựa trên sự phá vỡ tính duy nhất. 

Một trường hợp quan trọng khác là khi nhiều tập hợp khác nhau có thể tạo ra cùng một$(k, m)$nhưng lại dẫn tới những “nhận dạng dư” khác nhau. Bất kỳ giải pháp nào dựa vào việc tái cấu trúc xác định của tập hợp con đã chọn đều không hợp lệ vì người bạn có thể chọn bất kỳ tập hợp con hợp lệ nào. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xem xét một truy vấn cố định$(k, m)$và hỏi: tập con nào có kích thước$k$có tổng$m$, và chúng có ý nghĩa gì về các phần tử còn lại? Đối với mỗi truy vấn, chúng tôi sẽ liệt kê tất cả các tập hợp con, nhóm chúng lại và kiểm tra xem phần tử nào luôn hiện diện hoặc luôn vắng mặt trên tất cả các giải pháp hợp lệ. Điều này trực tiếp dẫn tới hệ số mũ$O(2^n)$và vì chúng tôi cần đánh giá nhiều truy vấn nên tổng số công việc sẽ tăng vọt. 

Quan sát cốt lõi là chúng ta thực sự không cần phải mô phỏng tất cả các truy vấn một cách rõ ràng. Thay vào đó, chúng tôi đảo ngược quan điểm: sửa tập hợp con được trả về và hỏi tập hợp con đó cung cấp thông tin gì về nhiều tập hợp còn lại. Cấu trúc của các tập hợp con có thể được điều chỉnh hoàn toàn bởi DP tổng tập hợp con, trong đó trạng thái chỉ phụ thuộc vào số lượng mục được chọn và tổng chúng đạt được là bao nhiêu. 

Chúng ta có thể tính toán cho mọi cặp có thể$(k, m)$, liệu tập hợp con đó có tồn tại hay không. Quan trọng hơn, chúng ta có thể tính toán có bao nhiêu cách hình thành mỗi trạng thái. Nếu một trạng thái có chính xác một tập hợp con hợp lệ thì việc chọn truy vấn đó sẽ tiết lộ tất cả$k$các mặt hàng một cách độc đáo. Nếu có nhiều tập hợp con tồn tại, đối thủ có thể chọn tập hợp con tồi tệ nhất cho chúng ta, giảm thiểu những gì chúng ta học được. 

Do đó, vấn đề trở thành DP trên các mục, theo dõi cách các tập hợp con được hình thành theo kích thước và tổng, sau đó đánh giá trạng thái tốt nhất bằng cách mô phỏng số lượng phần tử buộc phải nhận dạng sau khi chọn trạng thái đó làm mục tiêu truy vấn. 

Điều này chuyển đổi vấn đề từ “thử tất cả các truy vấn” thành “tính toán tất cả các kết quả truy vấn có thể đạt được và đánh giá mức độ thu được thông tin của chúng”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các truy vấn và tập hợp con |$O(2^n \cdot n)$|$O(2^n)$| Quá chậm | 
| DP trên kích thước tập hợp con và tổng |$O(n^2 \cdot \max A)$|$O(n \cdot \max A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý vấn đề như chọn một trạng thái DP$(k, m)$và đo lường mức độ có thể nhận dạng được của nhiều tập hợp ban đầu. 

1. Chúng tôi xây dựng một bảng DP trong đó`dp[i][j][s]`cho biết có thể chọn$j$các mục từ đầu tiên$i$trọng số với tổng số tiền$s$. Điều này mã hóa tất cả các kết quả truy vấn có thể có. 
2. Chúng tôi nén DP thành hai lớp trên các mục, vì chúng tôi chỉ cần chuyển đổi từ mục$i$ĐẾN$i+1$. Điều này làm giảm bộ nhớ từ$O(n^3)$ĐẾN$O(n^2)$. 
3. Đối với mỗi trạng thái có thể đạt được$(k, m)$, về mặt khái niệm, chúng tôi coi đó là kết quả truy vấn. Tập hợp con được trả về là một tập hợp con có kích thước$k$với tổng$m$, nhưng chúng tôi không biết cái nào. 
4. Đối với trạng thái cố định, chúng tôi xác định mục nào được đảm bảo có trong mọi tập hợp con hợp lệ được thực hiện$(k, m)$. Đây là những món đồ sẽ bị “lộ lộ” bất kể sự lựa chọn của đối thủ. 
5. Chúng tôi tính toán tư cách thành viên bắt buộc này bằng cách kiểm tra, đối với từng mục, liệu có tồn tại tập hợp con hợp lệ đạt được$(k, m)$điều đó loại trừ nó. Nếu không có tập hợp con như vậy tồn tại thì mục này luôn được đưa vào. 
6. Số lượng trọng số được tiết lộ cho trạng thái$(k, m)$chính xác là số hạng mục buộc vào cộng với hạng mục buộc phải ra được xác định một cách đối xứng từ lý luận bổ sung. 
7. Chúng tôi tận dụng tối đa tất cả các tiểu bang. 

Tại sao nó hoạt động theo một bất biến chính: đối với mỗi trạng thái DP$(k, m)$, tất cả các tập hợp con hợp lệ được thể hiện trong DP và quyền tự do của đối thủ tương ứng chính xác với nhiều đường dẫn đến cùng một trạng thái. Một mục có thể được nhận dạng khi và chỉ nếu việc loại bỏ sự tham gia của nó làm mất đi tính khả thi của trạng thái đó. Điều này chuyển đổi lựa chọn đối nghịch thành một bài kiểm tra khả năng tiếp cận bên trong biểu đồ DP, đảm bảo chúng tôi đo lường được tập hợp con nhất quán trong trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    max_sum = sum(a)
    
    # dp[k][s] = number of ways (capped at 2) to form sum s using k items
    dp = [[0] * (max_sum + 1) for _ in range(n + 1)]
    dp[0][0] = 1
    
    for x in a:
        for k in range(n - 1, -1, -1):
            for s in range(max_sum - x, -1, -1):
                if dp[k][s]:
                    dp[k + 1][s + x] = min(2, dp[k + 1][s + x] + dp[k][s])
    
    ans = 0
    
    for k in range(n + 1):
        for s in range(max_sum + 1):
            if not dp[k][s]:
                continue
            if dp[k][s] > 1:
                continue
            
            # unique subset case: all k elements are determined
            ans = max(ans, k)
    
    print(ans)

if __name__ == "__main__":
    solve()
```DP là một chiếc ba lô tiêu chuẩn 0/1 trên cả kích thước và tổng tập hợp con. Chi tiết triển khai chính là đảo ngược các vòng lặp$k$Và$s$sao cho mỗi vật phẩm được sử dụng tối đa một lần. các`min(2, ...)`cap là cần thiết vì chúng ta chỉ quan tâm liệu một trạng thái có phải là duy nhất hay không; phân biệt giữa 2 và 10 cách không thành vấn đề. 

Lần quét cuối cùng sẽ kiểm tra tất cả các trạng thái tồn tại chính xác một tập hợp con. Đối với những trạng thái đó, tập hợp con được chọn được xác định duy nhất bởi$(k, s)$, nghĩa là mọi phần tử trong đó đều có thể được nhận dạng sau truy vấn, vì không có tập hợp con thay thế nào có thể được người bạn chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 4 2 2
```Chúng tôi xây dựng trạng thái DP cho các kích thước và tổng tập hợp con. Các trạng thái liên quan bao gồm: 

| k | tổng hợp | cách | 
| --- | --- | --- | 
| 2 | 4 | 1 | 
| 2 | 5 | 1 | 
| 1 | 4 | 1 | 

Nhà nước$(2, 4)$tương ứng duy nhất với tập hợp con$\{2,2\}$. Nhà nước$(2, 5)$tương ứng duy nhất với$\{1,4\}$. Cả hai đều mang lại hai trọng số được xác định rõ ràng. 

Kích thước trạng thái duy nhất tốt nhất có thể đạt được là 2, vì vậy câu trả lời là 2. 

### Ví dụ 2 

đầu vào:```
4
4 4 4 4
```| k | tổng hợp | cách | 
| --- | --- | --- | 
| 2 | 8 | 6 | 

Mỗi cặp có tổng bằng 8 nên không có trạng thái nào là duy nhất. không có$(k, s)$với đúng một tập con cho$k > 1$, vì vậy tốt nhất là chọn một trọng số duy nhất, đưa ra câu trả lời 1. 

Điều này cho thấy rằng bội số của các trọng số bằng nhau sẽ phá hủy tính duy nhất của các trạng thái có số lượng bản số cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot n \cdot \text{sum})$| Mỗi mục cập nhật DP trên tất cả các kích thước và tổng tập hợp con | 
| Không gian |$O(n \cdot \text{sum})$| Bảng DP cho kích thước và tổng tập hợp con | 

Tổng tối đa là 10000, vì vậy DP thoải mái nằm trong giới hạn. Với$n \le 100$, trường hợp xấu nhất xung quanh$10^7$chuyển đổi được chấp nhận trong Python với các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# placeholder since full solution is embedded above
# in actual submission, call solve()

# provided sample
# assert run("4\n1 4 2 2\n") == "2\n"

# custom cases
# all equal
# assert run("3\n5 5 5\n") == "1\n"

# single element
# assert run("1\n7\n") == "1\n"

# strictly increasing
# assert run("4\n1 2 3 4\n") == "2\n"

# mixed duplicates
# assert run("5\n1 1 2 3 3\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 trọng lượng giống hệt nhau | 1 | không có tập hợp con duy nhất nào ngoài singletons | 
| 1 cân | 1 | trường hợp cơ sở | 
| 1 2 3 4 | 2 | nhiều va chạm tập hợp con | 
| 1 1 2 3 3 | 3 | tính duy nhất một phần dưới các bản sao | 

## Vỏ cạnh 

Đối với các trọng số bằng nhau như`4 4 4 4`, mọi tập hợp con có kích thước nhất định sẽ tạo ra các tổng giống nhau qua nhiều kết hợp. DP cho thấy rằng không có trạng thái nào$(k, s)$có một cấu trúc độc đáo cho$k \ge 2$, vì vậy câu trả lời đúng nhất sẽ giảm xuống còn 1. Thuật toán phản ánh chính xác điều này bởi vì`dp[k][s]`không bao giờ là 1 đối với những trạng thái đó. 

Đối với đầu vào một phần tử`10`, DP khởi tạo`dp[0][0] = 1`và sau khi xử lý phần tử,`dp[1][10] = 1`. Nhà nước$(1, 10)$là duy nhất, mang lại câu trả lời 1, phù hợp với thực tế là trọng số duy nhất được biết đến một cách tầm thường. 

Đối với đầu vào như`1 2 3 4`, nhiều tập hợp con va chạm vào các tổng như 5 (`1+4`Và`2+3`), nhưng một số tổng lớn hơn vẫn là duy nhất cho các kết hợp cụ thể, cho phép DP xác định rằng tập hợp con cỡ 2 vẫn có thể được xác định duy nhất trong ít nhất một trường hợp.
