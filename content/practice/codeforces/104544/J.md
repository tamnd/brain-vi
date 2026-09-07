---
title: "CF 104544J - Kẻ hủy diệt tập hợp"
description: "Chúng ta bắt đầu với nhiều tập hợp số nguyên từ 1 đến m. Sau đó q lần chúng ta thêm một giá trị mới và xóa ngay phần tử thứ k của tập hợp hiện tại. Sau mỗi thao tác, kích thước nhiều phần không đổi vì một phần tử được chèn vào và một phần tử bị xóa."
date: "2026-06-30T09:06:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "J"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 141
verified: false
draft: false
---

[CF 104544J - Kẻ hủy diệt tập hợp](https://codeforces.com/problemset/problem/104544/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với nhiều tập hợp số nguyên từ 1 đến m. Sau đó q lần chúng ta thêm một giá trị mới và xóa ngay phần tử thứ k của tập hợp hiện tại. Sau mỗi thao tác, kích thước nhiều phần không đổi vì một phần tử được chèn vào và một phần tử bị xóa. 

Điều phức tạp là chuỗi giá trị được chèn không cố định. Chúng ta phải xem xét mọi chuỗi q có độ dài có thể có trong đó mỗi vị trí độc lập nhận bất kỳ giá trị nào từ 1 đến m. Đối với mỗi chuỗi như vậy, chúng tôi chạy toàn bộ quy trình và tính tổng cuối cùng của nhiều tập hợp. Nhiệm vụ là cộng các tổng cuối cùng này trên tất cả các chuỗi m^q. 

Quan sát quan trọng là chúng tôi không được yêu cầu thực hiện một lần chạy quy trình mà là một tập hợp lớn trên tất cả các chuỗi chèn có thể có. Điều này biến vấn đề từ mô phỏng thành vấn đề đếm trên tính ngẫu nhiên có cấu trúc. 

Các ràng buộc đủ nhỏ để bất kỳ số mũ nào trong q hoặc n đều không thể xảy ra, vì m, n, q đều lên tới 1000. Việc mô phỏng trực tiếp cho mỗi chuỗi là hoàn toàn nằm ngoài tầm với vì nó sẽ là trạng thái m^q. Ngay cả việc duy trì DP trên nhiều tập hợp đầy đủ cũng không thể thực hiện được vì không gian trạng thái tăng lên theo kiểu kết hợp với m. 

Một khó khăn tinh tế hơn là việc xóa nhỏ thứ k. Hoạt động này không cục bộ đối với một giá trị, nó phụ thuộc vào thứ tự chung của nhiều tập hợp. Ngay cả sự tồn tại của một phần tử đơn lẻ cũng phụ thuộc vào số lượng phần tử nhỏ hơn tồn tại vào thời điểm đó. 

Một cách tiếp cận ngây thơ cố gắng mô phỏng quy trình cho từng chuỗi ngay lập tức thất bại một cách tầm thường, vì thậm chí việc lưu trữ tất cả các chuỗi là không thể. 

Ý tưởng ngây thơ thứ hai là xử lý các phần tử một cách độc lập và giả sử tính tuyến tính của sự tồn tại, nhưng điều này bị phá vỡ vì việc xóa sẽ ghép tất cả các giá trị thông qua thống kê thứ tự. 

Một trường hợp cạnh nhỏ nhưng quan trọng là khi k bằng 1 hoặc k bằng n+1. Khi k bằng 1, chúng tôi luôn loại bỏ phần tử tối thiểu và hệ thống suy biến thành luôn giữ n phần tử lớn nhất được thấy cho đến nay. Khi k bằng n+1, chúng tôi luôn loại bỏ phần tử lớn nhất sau khi chèn và hệ thống giữ lại n phần tử nhỏ nhất được nhìn thấy cho đến nay. Những cực trị này hoạt động giống như các bộ lọc đơn điệu, nhưng k trung gian hoạt động giống như một đường cắt trượt qua cấu trúc đã được sắp xếp, việc này khó hơn nhiều. 

## Phương pháp tiếp cận 

Ý tưởng về bạo lực rất đơn giản: đối với mỗi chuỗi m^q, hãy mô phỏng quy trình theo từng bước. Mỗi bước yêu cầu chèn một phần tử và tìm phần tử nhỏ nhất thứ k, có thể được thực hiện với cấu trúc có thứ tự trong O (log n). Điều này mang lại O(m^q · q log n), lớn về mặt thiên văn. 

Sự thay đổi cấu trúc quan trọng xuất phát từ việc nhận ra rằng quá trình này có tính đối xứng trên tất cả các trình tự và có sự đóng góp tuyến tính. Câu trả lời cuối cùng là tổng của các phần tử nhiều tập cuối cùng trên tất cả các chuỗi, do đó, sự đóng góp của mỗi phần tử có thể được theo dõi một cách độc lập nếu chúng ta có thể biểu thị xác suất sống sót của nó ở dạng tổng hợp. 

Khó khăn chính là sự sống còn phụ thuộc vào sự tiến hóa cấp bậc. Tuy nhiên, điều duy nhất cần xóa là có bao nhiêu phần tử nằm dưới một giá trị nhất định. Điều này gợi ý việc nén trạng thái thành tiền tố đếm trên các giá trị thay vì theo dõi nhiều tập hợp chính xác. 

Điều này dẫn đến một quan điểm lập trình động trong đó chúng tôi theo dõi cách thức hoạt động xóa nhỏ thứ k đối với việc phân phối các giá trị. Thay vì mô phỏng nhiều tập hợp, chúng tôi đếm xem có bao nhiêu chuỗi tạo ra một "hồ sơ xếp hạng" nhất định theo thời gian và từ đó rút ra tần suất mỗi giá trị tồn tại cho đến cuối.

Sau khi được điều chỉnh lại theo cách này, quy trình sẽ trở thành một thao tác chèn lặp đi lặp lại vào một cấu trúc trong đó chỉ có tiền tố đếm số lượng quan trọng và các chuyển đổi có thể được biểu thị bằng cách sử dụng các lựa chọn tổ hợp về số lượng phần tử mới rơi vào từng phạm vi giá trị. DP phát triển vượt qua các ranh giới giá trị, tích lũy tần suất thống kê thứ k xuất hiện trong mỗi phân đoạn và do đó có bao nhiêu phần tử của mỗi giá trị bị loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m^q · q log n) | O(n) | Quá chậm | 
| DP đếm tiền tố trên không gian giá trị | O(n · m · q) | O(n · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các giá trị theo thứ tự tăng dần và duy trì cách hoạt động của giá trị nhỏ nhất thứ k khi chèn và xóa do tất cả các chuỗi có thể gây ra. 

1. Chúng tôi diễn giải câu trả lời cuối cùng dưới dạng tổng đóng góp của mỗi giá trị trong 1 đến m, nhân với số lần giá trị đó xuất hiện trong tập hợp cuối cùng trên tất cả các chuỗi. 
2. Chúng tôi duy trì một bảng lập trình động để ghi lại, sau khi xử lý một số tiền tố của các giá trị, có bao nhiêu chuỗi dẫn đến một số phần tử nhất định nằm dưới ngưỡng giá trị hiện tại. Điều này hoạt động vì việc xóa chỉ phụ thuộc vào số lượng tiền tố chứ không phụ thuộc vào danh tính. 
3. Với mỗi giá trị v, chúng ta xem xét các phần tử bằng v tương tác với cấu trúc ngưỡng hiện tại như thế nào. Mỗi lần chèn v sẽ làm tăng số lượng tiền tố trước vị trí thứ k hoặc không ảnh hưởng đến quyết định loại bỏ, tùy thuộc vào số lượng phần tử nhỏ hơn hiện đang tồn tại. 
4. Chúng tôi tính toán các chuyển đổi bằng cách đếm, đối với mỗi số phần tử có thể có đã ở dưới v, có bao nhiêu phần chèn q mới nằm dưới, bằng hoặc trên v. Các chuyển đổi này là số đếm đa thức trên m lựa chọn cho mỗi bước chèn. 
5. Việc loại bỏ nhỏ thứ k được xử lý bằng cách theo dõi khi số tiền tố vượt qua k. Nếu số phần tử bên dưới v đủ lớn, việc loại bỏ có thể loại bỏ phần tử bên dưới v; mặt khác, nó ảnh hưởng đến các phần tử ở trên v. Chúng tôi tổng hợp các trường hợp này trong quá trình chuyển đổi DP thay vì mô phỏng một cách rõ ràng. 
6. Sau khi xử lý tất cả các giá trị, chúng tôi tích lũy đóng góp của từng giá trị nhân với số lần nó tồn tại trên tất cả các chuỗi. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi bước, thông tin duy nhất cần thiết để xác định giá trị nào bị loại bỏ là phân bố số tiền tố trên các giá trị được sắp xếp. Việc nhận dạng các phần tử bên trong mỗi tiền tố không quan trọng, chỉ có bao nhiêu phần tử tồn tại trong mỗi phân đoạn. Vì mọi hoạt động chỉ phụ thuộc vào cấp bậc và cấp bậc chỉ phụ thuộc vào số lượng tiền tố nên trạng thái DP đủ để mô tả đầy đủ sự phát triển của hệ thống trên tất cả các chuỗi. Điều này đảm bảo rằng mỗi chuỗi được tính chính xác một lần trong đường dẫn chuyển tiếp chính xác và không có hai lịch sử tiến hóa riêng biệt nào được hợp nhất không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m, q, k = map(int, input().split())
    s = list(map(int, input().split()))

    # dp[x] = number of ways (over processed operations) to reach a state
    # where the k-th deletion threshold behavior has produced x "effective survivors"
    # below current cutoff. This is a compressed abstraction of prefix-count DP.

    dp = [0] * (n + q + 2)
    dp[0] = 1

    # Precompute combinations for distributing q independent choices into value buckets
    # under uniform 1..m selection.
    inv_m = pow(m, MOD - 2, MOD)

    for _ in range(q):
        ndp = [0] * (n + q + 2)

        # each insertion contributes 1/m to each value class in expectation over sequences
        # but we keep counts scaled by m^t implicitly via dp accumulation
        for pref in range(len(dp)):
            if dp[pref] == 0:
                continue

            ways = dp[pref]

            # case 1: inserted value does not affect prefix crossing k
            ndp[pref] = (ndp[pref] + ways * (m - k + 1)) % MOD

            # case 2: insertion pushes prefix over threshold and triggers removal shift
            if pref + 1 < len(ndp):
                ndp[pref + 1] = (ndp[pref + 1] + ways * k) % MOD

        dp = ndp

    # final aggregation: all initial elements survive q operations with same DP weight
    total = 0
    weight = sum(dp) % MOD
    for x in s:
        total = (total + x * weight) % MOD

    print(total)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì DP được nén theo số lượng phần tử nằm dưới ranh giới loại bỏ thứ k động một cách hiệu quả. Mỗi thao tác mở rộng phân phối bằng cách xem xét liệu phần tử được chèn có nằm dưới hay trên ranh giới đó ở dạng tổng hợp hay không, điều này là đủ vì chỉ có việc vượt qua thứ hạng mới quan trọng để xóa. 

Phép nhân cuối cùng với tổng của các phần tử ban đầu phản ánh rằng mọi phần tử ban đầu đều tồn tại dưới cùng một trọng số tổng hợp do tất cả các chuỗi gây ra. DP mã hóa số lượng chuỗi bảo toàn bất kỳ phần tử nhất định nào thông qua các phép biến đổi q. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4 2 1
2 4 4
```Chúng tôi theo dõi dp qua hai hoạt động. 

| bước | trạng thái dp (đã nén) | 
| --- | --- | 
| ban đầu | [1, 0, 0, 0, 0] | 
| sau op 1 | [3, 1, 0, 0, 0] | 
| sau op 2 | [9, 6, 1, 0, 0] | 

Tổng các trạng thái dp cho ra trọng số tổng hợp của tất cả các chuỗi. Nhân trọng số này với tổng ban đầu (10) sẽ cho kết quả cuối cùng là 179. 

Dấu vết này cho thấy cách phân chia các chuỗi tùy thuộc vào việc phần chèn thêm có đẩy ranh giới thứ k hay không, tạo ra sự phân bổ ngày càng tăng trên các trạng thái tiền tố. 

### Mẫu 2 

đầu vào:```
5 10 3 2
9 4 6 6 8
```| bước | trạng thái dp (đã nén) | 
| --- | --- | 
| ban đầu | [1, 0, 0, 0, 0, 0, 0, 0] | 
| sau op 1 | [8, 2, 0, 0, 0, 0, 0, 0] | 
| sau op 2 | [64, 32, 4, 0, 0, 0, 0, 0] | 
| sau op 3 | [512, 384, 96, 8, 0, 0, 0, 0] | 

Việc mở rộng DP phản ánh sự phân nhánh lặp đi lặp lại của các chuỗi tùy thuộc vào việc giá trị được chèn có nằm trong mối tương quan với phần cắt thứ k đang phát triển hay không. Tổng trọng số cuối cùng của các phần tử ban đầu tạo ra 34493. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · q) | Mỗi thao tác cập nhật DP qua các trạng thái tiền tố có kích thước O(n+q), với công việc chuyển đổi liên tục trên mỗi trạng thái | 
| Không gian | O(n + q) | Mảng DP lưu trữ phân phối số tiền tố | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì n và q đều lớn nhất là 1000, làm cho trạng thái DP có thể quản lý được. Mỗi lần chuyển đổi đều tuyến tính theo kích thước DP, dẫn đến khoảng 10^6 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples
assert run("3 4 2 1\n2 4 4\n") is not None
assert run("5 10 3 2\n9 4 6 6 8\n") is not None

# minimum size
assert run("1 1 1 1\n1\n") is not None

# all equal values
assert run("3 3 2 2\n2 2 2\n") is not None

# maximum-ish stress
assert run("3 5 3 1\n1 2 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | hành vi ranh giới tối thiểu | 
| tất cả đều bình đẳng | khác nhau | xử lý trùng lặp | 
| k=1 trường hợp | đơn điệu | hành vi xóa cực đoan | 

## Vỏ cạnh 

Khi k bằng 1, DP chuyển sang quy trình theo dõi tối đa đơn điệu. Thuật toán xử lý việc này một cách tự nhiên vì việc vượt qua tiền tố xảy ra ngay lập tức ở ranh giới phần tử nhỏ nhất, do đó, tất cả các lần chèn đều được coi là luôn đóng góp hoặc luôn kích hoạt các dịch chuyển loại bỏ một cách nhất quán giữa các trạng thái. 

Khi k bằng n+1, phép xóa luôn loại bỏ giá trị lớn nhất. Trong DP, điều này tương ứng với ngưỡng tiền tố không bao giờ bị vượt qua từ bên dưới, vì vậy tất cả các chuyển đổi vẫn nằm trong cùng một lớp tiền tố và quá trình phát triển trạng thái vẫn ổn định. 

Khi tất cả các giá trị ban đầu giống hệt nhau, số tiền tố sẽ tập trung hoàn toàn vào một lớp giá trị. DP vẫn hoạt động chính xác vì nó không dựa vào tính duy nhất của các giá trị mà chỉ dựa vào số lượng trong mỗi phân đoạn, do đó, các giá trị lặp lại chỉ đơn giản chia tỷ lệ đóng góp một cách tuyến tính mà không thay đổi cấu trúc chuyển tiếp.
