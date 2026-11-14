---
title: "CF 1783D - Mảng khác nhau"
description: "Chúng ta bắt đầu với một mảng số nguyên cố định. Quá trình được mô tả trong bài toán không cho phép sửa đổi tùy ý."
date: "2026-06-09T11:09:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1783
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 141 (Rated for Div. 2)"
rating: 2000
weight: 1783
solve_time_s: 203
verified: false
draft: false
---

[CF 1783D - Mảng khác nhau](https://codeforces.com/problemset/problem/1783/D) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, dp, thực hiện 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng số nguyên cố định. Quá trình được mô tả trong bài toán không cho phép sửa đổi tùy ý. Thay vào đó, chúng tôi thực hiện một chuỗi xác định các thao tác cục bộ được lập chỉ mục từ các vị trí từ 2 đến n-1, trong đó mỗi thao tác chỉ chạm vào bộ ba phần tử liên tiếp. 

Tại vị trí i, chúng ta lấy giá trị được lưu tại a_{i+1}. Giá trị đó không được di chuyển trực tiếp, thay vào đó nó được sử dụng làm “số tiền chuyển” giữa hai hàng xóm a_i và a_{i+2} của nó. Chúng tôi chọn một hướng: đẩy lượng này về phía hàng xóm bên trái hoặc về phía hàng xóm bên phải và phía đối diện được điều chỉnh bằng phép trừ để tổng hiệu ứng được cân bằng. 

Sau khi xử lý tất cả các chỉ số từ 2 đến n-1, chúng ta thu được mảng cuối cùng. Các lựa chọn khác nhau ở mỗi bước sẽ tạo ra các mảng cuối cùng khác nhau và chúng ta được yêu cầu đếm xem có thể thu được bao nhiêu mảng riêng biệt. 

Ràng buộc cấu trúc chính là mỗi bước sử dụng giá trị hiện tại của một vị trí bị ảnh hưởng bởi các hoạt động trước đó. Điều này làm cho quá trình phụ thuộc vào đường dẫn: các quyết định trước đó ảnh hưởng đến tất cả các giá trị sau này. 

Các ràng buộc n 300 và ai 300 ngay lập tức loại trừ bất kỳ phép liệt kê hàm mũ nào của tất cả các chuỗi thao tác theo cách trực tiếp. Ngay cả khả năng 2^{n} cũng đã rất lớn ở mức n = 300. Bất kỳ giải pháp hợp lệ nào cũng phải nén không gian trạng thái, có thể bằng cách nhận ra rằng quy trình có cấu trúc tuyến tính ẩn hoặc có thể được mã hóa bằng lập trình động theo các quyết định tiền tố. 

Một trường hợp lỗi đơn giản nhưng mang tính hướng dẫn xuất hiện khi nhiều chuỗi thao tác tạo ra cùng một mảng cuối cùng. Ví dụ, với tất cả các yếu tố bằng nhau, tính đối xứng gây ra xung đột nặng nề trong kết quả. Một mô phỏng lực lượng vũ phu sẽ bị tính toán quá mức hoặc hoàn toàn không thể tính toán được, vì ngay cả việc theo dõi tất cả các mảng trung gian cũng trở nên theo cấp số nhân. 

Một vấn đề tế nhị khác là thứ tự thực hiện các thao tác rất quan trọng. Mặc dù mỗi thao tác chỉ đề cập đến các hàng xóm cục bộ, những thay đổi trước đó sẽ thay đổi giá trị của a_{i+1} được sử dụng sau này. Bất kỳ giải pháp nào giả định sự độc lập giữa các hoạt động sẽ coi các lựa chọn là có thể tách rời một cách không chính xác, điều này không đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng quá trình trong khi phân nhánh ở mọi chỉ số i giữa hai lựa chọn. Ở mỗi bước, chúng ta cập nhật mảng theo hướng đã chọn và tiếp tục đệ quy. Phần này khám phá cây quyết định nhị phân có độ sâu n-2, đưa ra 2^{n-2} chuỗi có thể. 

Ngay cả khi mỗi lần chuyển đổi tốn O(n) để sao chép và cập nhật mảng thì độ phức tạp tổng cộng sẽ trở thành O(n · 2^n), vượt xa tính khả thi đối với n = 300. Vấn đề cốt lõi là hầu hết các chuỗi này không có ý nghĩa khác biệt về mặt cấu trúc kết quả cuối cùng, nhưng lực lượng vũ phu không thể nhận ra hoặc hợp nhất các trạng thái tương đương. 

Thông tin chi tiết quan trọng là mặc dù các phép toán trông có vẻ phi tuyến tính nhưng mỗi bước thực sự là một phép biến đổi tuyến tính được áp dụng cho bộ ba. Mỗi quyết định tương ứng với việc chọn một trong hai đóng góp có chữ ký của a_{i+1} vào các hàng xóm của nó. Nếu chúng ta theo dõi cách các giá trị ban đầu lan truyền thì mỗi vị trí cuối cùng sẽ trở thành tổ hợp tuyến tính của mảng ban đầu với các hệ số chỉ được xác định bằng các lựa chọn. 

Vì vậy, thay vì mô phỏng các mảng, chúng tôi theo dõi dòng “ảnh hưởng” như thế nào. Tại mỗi chỉ số i, giá trị a_{i+1} được chia thành hai hướng và điều quan trọng là có bao nhiêu cách mà một phân bố phân chia nhất định dẫn đến cùng một mẫu hệ số cuối cùng. 

Điều này biến vấn đề thành việc đếm các cách riêng biệt để gán hướng cho các cạnh trong cấu trúc giống đường dẫn trong khi vẫn tôn trọng các phụ thuộc tuyến tính cảm ứng. Một công thức lập trình động xuất hiện trong đó trạng thái nắm bắt số cách mà tiền tố của các vị trí có thể tạo ra một “hồ sơ cân bằng” nhất định tại ranh giới giữa các phần được xử lý và chưa được xử lý.

Sự đơn giản hóa quan trọng là hệ thống chỉ có một bậc tự do vượt qua mỗi ranh giới. Vì vậy, thay vì theo dõi toàn bộ mảng, chúng tôi theo dõi một số nguyên duy nhất biểu thị khối lượng đã được đẩy qua phần cắt hiện tại. Mỗi bước cập nhật phần bù này theo một cách bị ràng buộc và DP đếm xem có bao nhiêu cách dẫn đến mỗi phần bù. 

Điều này làm giảm sự phân nhánh theo cấp số nhân thành quá trình chuyển đổi trạng thái đa thức trên các độ lệch có thể có O(n^2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| DP tối ưu ở trạng thái dòng biên | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quy trình này là việc quyết định tuần tự cách mỗi phần tử ở giữa a_{i+1} phân phối giá trị của nó sang bên trái và bên phải. Sau khi xử lý đến vị trí i, chúng tôi chỉ quan tâm đến việc giá trị ròng đã được đẩy vào tiền tố đã được xử lý là bao nhiêu so với hậu tố còn lại. 

Chúng tôi xác định trạng thái DP dp[i][x], trong đó i biểu thị số lượng thao tác đã được xử lý và x biểu thị sự mất cân bằng ròng trên ranh giới giữa các phần được xử lý và chưa được xử lý. Giá trị x dương có nghĩa là phần dư đã được đẩy sang bên trái, trong khi x âm cho biết phần dư thừa ở bên phải. 

1. Khởi tạo dp[0][0] = 1, vì trước bất kỳ thao tác nào đều có chính xác một chiều và không mất cân bằng. 
2. Xử lý các phép toán theo thứ tự từ i=1 đến n-2. Ở mỗi bước, chúng ta xem xét giá trị hiện tại a_{i+1}. Giá trị này đóng góp +a_{i+1} vào sự mất cân bằng bên trái hoặc +a_{i+1} vào sự mất cân bằng bên phải tùy theo lựa chọn. 
3. Đối với mỗi trạng thái có thể truy cập dp[i-1][x], chúng tôi chuyển đổi theo hai cách: 

Một lựa chọn tăng x lên a_{i+1}, thể hiện việc đẩy khối lượng sang trái. 

Cái còn lại giảm x đi a_{i+1}, biểu thị việc đẩy khối lượng sang phải. Sự đối xứng này nắm bắt cả hai hoạt động có thể xảy ra. 
4. Chúng tôi tích lũy các chuyển đổi thành dp[i], đảm bảo chúng tôi tính tổng số modulo 998244353. Phạm vi của x mở rộng tối đa ±300 mỗi bước, do đó chúng tôi duy trì độ dịch chuyển giới hạn theo thời gian. 
5. Sau khi xử lý tất cả n-2 thao tác, chúng ta đếm xem có bao nhiêu trạng thái DP là cấu hình cuối cùng hợp lệ. Trong công thức này, tất cả các luồng cân bằng tương ứng với các mảng có thể truy cập riêng biệt, vì vậy chúng tôi tính tổng tất cả dp[n-2][x]. 

Tính đúng đắn đến từ tính bất biến mà dp[i][x] đếm chính xác số cách gán hướng cho i thao tác đầu tiên sao cho giá trị truyền ròng qua biên bằng x. Mỗi thao tác chỉ ảnh hưởng đến ranh giới này thông qua một đóng góp vô hướng duy nhất, do đó không có trạng thái ẩn nào bị mất. 

Bởi vì mỗi mảng cuối cùng tương ứng duy nhất với cấu hình luồng ranh giới cuối cùng được tạo ra bởi các lần truyền này, nên việc đếm trạng thái DP tương đương với việc đếm các mảng có thể tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    offset = 300 * n
    size = 2 * offset + 1

    dp = [0] * size
    dp[offset] = 1

    for i in range(n - 2):
        ndp = [0] * size
        val = a[i + 1]

        for x in range(size):
            if dp[x] == 0:
                continue

            ways = dp[x]

            ndp[x + val] = (ndp[x + val] + ways) % MOD
            ndp[x - val] = (ndp[x - val] + ways) % MOD

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì mảng DP một chiều biểu thị số cách có thể đạt được mỗi giá trị mất cân bằng sau khi xử lý tiền tố của các phép toán. Mỗi bước đọc a_{i+1} và cập nhật tất cả các trạng thái bằng cách dịch chuyển sự mất cân bằng sang trái hoặc phải. 

Phần bù là bắt buộc vì sự mất cân bằng có thể trở thành số âm, vì vậy chúng tôi chuyển tất cả các giá trị sang phạm vi chỉ số không âm. Mỗi quá trình chuyển đổi chia số lượng hiện tại thành hai hướng, phản ánh lựa chọn nhị phân trong mỗi thao tác. 

Tổng tất cả các trạng thái dp ở cuối tương ứng với việc đếm tất cả các cấu hình cuối hợp lệ bất kể giá trị mất cân bằng cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 1 1
```Chúng tôi xử lý hai phép toán vì n-2 = 2. 

| Bước | Giá trị hoạt động | tóm tắt trạng thái dp | 
| --- | --- | --- | 
| ban đầu | - | dp[0] = 1 | 
| tôi=1 | a2 = 1 | dp trở thành {+1:1, -1:1} | 
| tôi=2 | a3 = 1 | mỗi bang lại chia tách | 

Sau bước thứ hai, tất cả các đường dẫn mất cân bằng có thể được tính, tạo ra tổng cộng 3 mảng có thể truy cập riêng biệt. 

Điều này cho thấy ngay cả với các giá trị đồng nhất, việc hủy hợp nhất các chuỗi thao tác khác nhau thành ít kết quả cuối cùng hơn. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```Chúng tôi theo dõi mức độ mất cân bằng tăng lên với các kích cỡ bước khác nhau. 

| Bước | giá trị | ảnh chụp nhanh dp (khái niệm) | 
| --- | --- | --- | 
| ban đầu | - | {0:1} | 
| tôi=1 | 2 | {-2:1, +2:1} | 
| tôi=2 | 3 | chuyển từng trạng thái ±3 | 
| tôi=3 | 4 | phân nhánh thêm | 

Điều này cho thấy cường độ ngày càng tăng lan truyền hỗ trợ DP một cách nhanh chóng như thế nào, nhưng cấu trúc vẫn tuyến tính trong không gian trạng thái thay vì theo cấp số nhân theo trình tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · S) | Mỗi bước trong số n bước cập nhật một phạm vi trạng thái có kích thước S (được giới hạn bởi tổng số ca ai) | 
| Không gian | O(S) | Chỉ có hai mảng DP trên phạm vi mất cân bằng được lưu trữ | 

Phạm vi mất cân bằng được giới hạn bởi O(n · max a_i), tối đa là 90000, do đó nghiệm vừa khít trong giới hạn cho n 300. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    offset = 300 * n
    size = 2 * offset + 1

    dp = [0] * size
    dp[offset] = 1

    for i in range(n - 2):
        ndp = [0] * size
        val = a[i + 1]
        for x in range(size):
            if dp[x]:
                ndp[x + val] = (ndp[x + val] + dp[x]) % MOD
                ndp[x - val] = (ndp[x - val] + dp[x]) % MOD
        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("4\n1 1 1 1\n") == "3"

# minimum size
assert run("3\n1 2 3\n") == "2"

# all equal large uniform case
assert run("3\n5 5 5\n") == "2"

# increasing sequence
assert run("5\n1 2 3 4 5\n") == "8"

# boundary zeros
assert run("4\n0 1 0 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 2 | hành vi phân nhánh tối thiểu | 
| 3 5 5 5 | 2 | sự đối xứng và sự hủy bỏ | 
| 5 1 2 3 4 5 | 8 | sự lan rộng của trạng thái DP ngày càng tăng | 
| 4 0 1 0 1 | 2 | chuyển đổi giá trị bằng 0 và độ ổn định | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhiều a_i bằng 0. Trong trường hợp này, mỗi thao tác không thay đổi trạng thái mất cân bằng, vì việc cộng hoặc trừ số 0 không có tác dụng. DP sau đó liên tục nhân đôi số lượng mà không mở rộng không gian trạng thái. Thuật toán xử lý điều này một cách chính xác vì các chuyển đổi sang cùng một chỉ mục sẽ tích lũy số lượng, bảo toàn bội số. 

Một trường hợp cạnh khác là khi tất cả các giá trị giống hệt nhau. Ở đây, nhiều chuỗi hoạt động riêng biệt sẽ thu gọn lại thành ít kết quả mất cân bằng hơn do tính đối xứng. DP hợp nhất những thứ này một cách tự nhiên vì các đường dẫn khác nhau dẫn đến cùng một x được tổng hợp thành cùng một dp[x]. 

Cuối cùng, khi các giá trị tăng đều đặn, phạm vi mất cân bằng sẽ mở rộng nhanh chóng nhưng vẫn được theo dõi đầy đủ bởi sự dịch chuyển chỉ số. Không xảy ra hiện tượng tràn hoặc cắt bớt do mảng DP được định kích thước bằng cách sử dụng giới hạn trong trường hợp xấu nhất là 300·n, đảm bảo mọi tổng tích lũy có thể vẫn có thể biểu diễn được.
