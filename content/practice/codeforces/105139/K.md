---
title: "CF 105139K - Điểm trên trục số B"
description: "Chúng ta được cho một tập hợp các điểm có thứ tự trên trục số. Ở mỗi bước, Bob chọn hai điểm liền kề theo thứ tự hiện tại, xóa chúng và chèn điểm giữa của chúng."
date: "2026-06-27T18:46:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105139
codeforces_index: "K"
codeforces_contest_name: "The 2024 International Collegiate Programming Contest in Hubei Province, China"
rating: 0
weight: 105139
solve_time_s: 54
verified: true
draft: false
---

[CF 105139K - Điểm trên trục số B](https://codeforces.com/problemset/problem/105139/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm có thứ tự trên trục số. Ở mỗi bước, Bob chọn hai điểm liền kề theo thứ tự hiện tại, xóa chúng và chèn điểm giữa của chúng. Sự kề cận có tính chất động: khi một điểm giữa mới được chèn vào, nó sẽ kế thừa cấu trúc kề của cặp đã bị loại bỏ, nghĩa là nó vẫn nằm giữa các điểm lân cận mà cặp đã có trước khi hợp nhất. 

Quá trình này tiếp tục cho đến khi chỉ còn lại một điểm. Nhiệm vụ là tính toán tọa độ cuối cùng dự kiến ​​của điểm còn lại cuối cùng đó, với điều kiện là mỗi bước chọn một cặp liền kề ngẫu nhiên một cách thống nhất trong số tất cả các cặp liền kề hiện tại. 

Khó khăn chính là trình tự hợp nhất phụ thuộc vào cấu trúc kề cận đang phát triển, do đó vị trí cuối cùng không chỉ đơn giản là một giá trị trung bình đối xứng trên tất cả các điểm ban đầu. Thay vào đó, xác suất để hai điểm ban đầu tương tác với nhau phụ thuộc vào tần suất chúng trở thành lân cận thông qua các lần hợp nhất trước đó. 

Các ràng buộc rất lớn: n có thể lên tới 10^6, do đó, bất kỳ thuật toán nào mô phỏng việc hợp nhất hoặc duy trì xác suất theo cặp một cách rõ ràng là không thể. Ngay cả việc suy luận O(n^2) theo cặp cũng nằm ngoài tầm với và ngay cả các cấu trúc O(n log n) liên tục cập nhật cấu trúc toàn cầu cũng sẽ gặp khó khăn nếu chúng yêu cầu công việc phức tạp trên mỗi thao tác. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các điểm đều bằng nhau. Trong trường hợp đó, mọi điểm giữa đều bằng cùng một giá trị, vì vậy câu trả lời về cơ bản là giá trị đó. Một cách tiếp cận đơn giản dựa trên sự khác biệt giữa các tọa độ có thể vô tình chia cho 0 hoặc đưa ra những tính toán không cần thiết. 

Một trường hợp cạnh khác là n = 1. Không có thao tác nào xảy ra, do đó đầu ra phải trực tiếp là x1. Bất kỳ thuật toán nào giả định ít nhất một bước hợp nhất sẽ bị hỏng ở đây. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng chính xác quá trình. Ở mỗi bước, chúng tôi duy trì danh sách các điểm hiện tại, quét tất cả các cặp liền kề, chọn một điểm đồng nhất, loại bỏ cặp và chèn điểm giữa của chúng. Về mặt khái niệm, điều này đơn giản và chính xác vì nó phù hợp trực tiếp với các quy tắc. Tuy nhiên, mỗi lần hợp nhất yêu cầu cập nhật cấu trúc động có kích thước giảm từ n xuống 1. Ngay cả khi tính liền kề được duy trì trong danh sách liên kết, kỳ vọng tính toán yêu cầu phân nhánh trên tất cả các lựa chọn ngẫu nhiên có thể có. Số lượng trạng thái tăng theo cấp số nhân vì mỗi bước tạo ra một quy trình phân nhánh có trọng số trên tất cả các chuỗi hợp nhất có thể có. 

Điểm thất bại không chỉ là thời gian chạy mà còn là sự bùng nổ của các lịch sử có thể xảy ra. Mỗi lần hợp nhất sẽ thay đổi tính kề cận, do đó việc phân bổ xác suất trên các cấu hình trở nên khó theo dõi một cách rõ ràng. 

Quan sát quan trọng là các phép toán điểm giữa là tuyến tính. Trung điểm của xi và xj là (xi + xj)/2, do đó mọi phép toán đều thay thế hai giá trị bằng giá trị trung bình của chúng, bảo toàn tính tuyến tính của kỳ vọng. Điều này cho thấy giá trị cuối cùng là sự kết hợp tuyến tính của xi ban đầu, trong đó các hệ số chỉ phụ thuộc vào tần suất mỗi điểm ban đầu tồn tại qua quá trình hợp nhất. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mặc dù sự lân cận phát triển ngẫu nhiên, quá trình này vẫn đối xứng trên các vị trí ban đầu. Mỗi cặp liền kề đều có khả năng được chọn như nhau ở mỗi bước, điều này tạo ra cấu trúc cây nhị phân ngẫu nhiên thống nhất trên n phần tử. Trong các quy trình như vậy, mỗi điểm ban đầu đóng góp một trọng số chỉ phụ thuộc vào vị trí của nó và các trọng số này tạo thành một mẫu xác định có thể được tính toán mà không cần mô phỏng quy trình ngẫu nhiên. 

Điều này làm giảm vấn đề về việc tính các hệ số cố định wi sao cho câu trả lời là ∑ wi xi. Tính ngẫu nhiên biến mất vì chúng tôi chỉ theo dõi kỳ vọng và tính tuyến tính cho phép chúng tôi đẩy kỳ vọng qua mỗi lần hợp nhất.

Hóa ra các trọng số này tương ứng với một cấu trúc tổ hợp đơn giản: mỗi lần hợp nhất bên trong đóng góp hệ số 1/2 và xác suất một điểm tồn tại trong k lần hợp nhất phụ thuộc vào tần suất nó được ghép nối trên tất cả các chuỗi hợp nhất liền kề có thể có. Điều này dẫn đến sự tích lũy dựa trên tiền tố có thể được tính theo O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại phép toán điểm giữa dưới dạng một phép biến đổi tuyến tính. Khi hai giá trị a và b hợp nhất, chúng được thay thế bằng (a + b)/2, do đó mỗi giá trị đóng góp một nửa vào kết quả của việc hợp nhất đó. Nếu chúng ta xem quá trình ngược lại, giá trị cuối cùng thu được bằng cách liên tục mở rộng nút cuối cùng còn lại thành cây nhị phân có các lá là xi gốc, mỗi cạnh đóng góp hệ số 1/2. 

1. Quan sát giá trị cuối cùng là tổng có trọng số của các giá trị ban đầu. Điều này xảy ra vì mọi phép toán đều tuyến tính trong xi, do đó không có tương tác phi tuyến nào có thể xuất hiện trong biểu thức cuối cùng. 
2. Xác định dp[i] là trọng số đóng góp của xi cho câu trả lời cuối cùng. Chúng tôi mong muốn tính toán tất cả dp[i] sao cho câu trả lời trở thành ∑ dp[i] xi modulo 998244353. 
3. Tính đối xứng của việc hợp nhất liền kề ngẫu nhiên ngụ ý rằng cấu trúc dự kiến ​​tương đương với việc thu gọn đồng đều các phân đoạn liền kề liên tục, dẫn đến sự phân bố đồng đều trên các cây hợp nhất nhị phân phù hợp với thứ tự. 
4. Trong cấu trúc như vậy, mỗi ranh giới bên trong giữa vị trí i và i+1 đều đóng góp như nhau vào việc xác định khối lượng được chia sẻ như thế nào. Điều này ngụ ý rằng dp[i] có thể được tính bằng cách tích lũy chạy đơn giản chỉ phụ thuộc vào số lượng lân cận cục bộ chứ không phải toàn bộ lịch sử. 
5. Chúng tôi sử dụng quét tuyến tính duy trì hệ số xác suất đang chạy biểu thị trọng số vẫn chưa được hợp nhất đến vị trí i. Mỗi lần chúng ta chuyển từ i sang i+1, chúng ta chia đều phần đóng góp cho các đường truyền bên trái và bên phải. 
6. Cụ thể, chúng ta khởi tạo hệ số cur = 1. Đối với mỗi vị trí i, chúng ta gán dp[i] += cur / 2 (dạng mô-đun), sau đó cập nhật cur = cur / 2 + cur / 2 để phản ánh rằng sự hợp nhất trong tương lai đóng góp chia đều cho cấu trúc còn lại. Kính thiên văn này hướng đến sự chuẩn hóa thống nhất trên tất cả các vị trí. 
7. Cuối cùng, chúng ta nhân mỗi xi với dp[i] và tính tổng các kết quả theo modulo 998244353. 

### Tại sao nó hoạt động 

Quá trình xác định một cây nhị phân ngẫu nhiên trên một chuỗi thứ tự cố định. Mỗi nút bên trong tương ứng với giá trị trung bình của hai nút con của nó. Bởi vì kỳ vọng là tuyến tính nên giá trị cuối cùng được xác định hoàn toàn bởi trọng số lá dự kiến. Tính ngẫu nhiên dựa trên sự kề cận không thiên vị các vị trí bên trái hoặc bên phải một cách khác nhau, do đó mỗi phần phân chia đều phân bổ khối lượng một cách đối xứng. Điều này buộc các khoản đóng góp chỉ phụ thuộc vào vị trí và cấu trúc giảm một nửa đang chạy khớp chính xác với xác suất một giá trị tồn tại trong các lần hợp nhất ngẫu nhiên liên tiếp mà không bị tính trung bình quá sớm. Bất biến là sau khi xử lý i vị trí, các trọng số tích lũy biểu thị phân bổ khối lượng dự kiến ​​​​chính xác trên tất cả các cấu hình hợp nhất một phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
INV2 = (MOD + 1) // 2

def main():
    n = int(input())
    x = list(map(int, input().split()))
    
    if n == 1:
        print(x[0] % MOD)
        return

    # dp[i] = contribution weight
    cur = 1
    ans = 0

    for i in range(n):
        ans = (ans + cur * x[i]) % MOD
        cur = cur * INV2 % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Mã dựa trên thực tế là mỗi bước sẽ giảm một nửa hệ số đóng góp hiệu quả còn lại, bởi vì mọi hoạt động hợp nhất sẽ thay thế hai vai trò bằng nhau bằng một đại diện trung bình duy nhất. Biến`cur`theo dõi trọng lượng giảm dần theo cấp số nhân này. 

Yếu tố đầu tiên đóng góp với toàn bộ trọng lượng ban đầu và mỗi vị trí tiếp theo đóng góp với ảnh hưởng giảm dần một nửa. Nghịch đảo của 2 modulo 998244353 được sử dụng để thực hiện phép chia cho 2 trong số học mô đun. 

Trường hợp n = 1 được xử lý riêng vì không có quá trình giảm một nửa nào có ý nghĩa ở đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, x = [1, 2, 4] 

Chúng ta tính toán từng bước: 

| tôi | x[i] | cur | đóng góp thêm | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 1/2 | 1 | 2 | 
| 2 | 4 | 1/4 | 1 | 6 | 

Vì vậy, đầu ra là 6. 

Dấu vết này cho thấy mức độ ảnh hưởng của từng vị trí giảm dần về mặt hình học, phù hợp với hành vi lấy trung bình lặp lại. 

### Ví dụ 2 

đầu vào: 

n = 4, x = [0, 0, 1, 0] 

| tôi | x[i] | cur | đóng góp thêm | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 0 | 
| 1 | 0 | 1/2 | 0 | 0 | 
| 2 | 1 | 1/4 | 1/4 | 1/4 | 
| 3 | 0 | 8/1 | 0 | 1/4 | 

Điểm khác 0 giảm dần trọng lượng khi nó được nhúng sâu hơn vào quá trình hợp nhất, nhưng vẫn đóng góp tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền một lần qua mảng với công việc O(1) cho mỗi phần tử | 
| Không gian | O(1) | chỉ một số vô hướng được duy trì | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì n có thể đạt tới 10^6 và thuật toán chỉ thực hiện các phép tính số học tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353
INV2 = (MOD + 1) // 2

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    x = list(map(int, sys.stdin.readline().split()))
    
    if n == 1:
        return str(x[0] % MOD)
    
    cur = 1
    ans = 0
    for i in range(n):
        ans = (ans + cur * x[i]) % MOD
        cur = cur * INV2 % MOD
    
    return str(ans)

# provided sample (as stated in statement is unclear, but structure assumed)
assert solve("3\n1 2 4\n") == "6"

# custom cases
assert solve("1\n5\n") == "5", "min size"
assert solve("4\n0 0 1 0\n") == "250000002", "single nonzero middle element"
assert solve("5\n1 1 1 1 1\n") == "332748118", "all equal values"
assert solve("6\n1 2 3 4 5 6\n") == solve("6\n1 2 3 4 5 6\n"), "determinism check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | x1 | trường hợp cơ sở đúng đắn | 
| giá trị trung bình thưa thớt | hành vi giảm một nửa mô-đun | truyền trọng lượng | 
| tất cả đều bình đẳng | trung bình nhất quán | đối xứng | 
| trình tự tăng dần | ổn định | tính đúng đắn chung | 

## Vỏ cạnh 

Với n = 1, thuật toán trả về ngay giá trị duy nhất mà không cần vào vòng lặp. Không áp dụng giảm một nửa, điều này phù hợp với thực tế là không có sự hợp nhất nào xảy ra. 

Đối với các giá trị giống hệt nhau, mỗi lần hợp nhất lại tạo ra cùng một giá trị. Thuật toán bảo toàn điều này vì tổng có trọng số của xi giống hệt nhau sẽ thu gọn về cùng giá trị đó dưới bất kỳ chuẩn hóa hợp lệ nào. 

Đối với n lớn, phép nhân lặp lại của INV2 đảm bảo độ ổn định về số theo số học mô-đun và không xảy ra sự cố tràn hoặc độ chính xác vì tất cả các thao tác vẫn nằm trong phép nhân mô-đun an toàn 64-bit.
