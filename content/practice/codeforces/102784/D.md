---
title: "CF 102784D - Ghost-or-Treat"
description: "Chúng ta có một dòng ma, trong đó mỗi con ma có một số nguyên tuổi. Một động thái bao gồm việc chọn hai hồn ma lân cận có độ tuổi khác nhau. Con ma lớn hơn đánh bại con ma trẻ hơn và vẫn ở trong hàng ngũ, nhưng việc sống sót khiến nó già đi một tuổi. Con ma trẻ biến mất."
date: "2026-07-27T19:54:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 81
verified: true
draft: false
---

[CF 102784D - Ghost-or-Treat](https://codeforces.com/problemset/problem/102784/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một dòng ma, trong đó mỗi con ma có một số nguyên tuổi. Một động thái bao gồm việc chọn hai hồn ma lân cận có độ tuổi khác nhau. Con ma lớn hơn đánh bại con ma trẻ hơn và vẫn ở trong hàng ngũ, nhưng việc sống sót khiến nó già đi một tuổi. Con ma trẻ biến mất. 

Câu hỏi đặt ra là liệu Boo có thể chọn các trận chiến theo thứ tự nào đó để cuối cùng chỉ còn lại một hồn ma hay không. 

Dữ liệu đầu vào cung cấp số lượng ma theo sau là độ tuổi của chúng theo thứ tự hiện tại. Đầu ra là`YES`nếu tồn tại một chuỗi trận đánh nào đó mà chỉ để lại đúng một con ma, và`NO`nếu không thì. 

Ràng buộc đủ nhỏ để chúng ta không cần cấu trúc dữ liệu nâng cao. Với tối đa 10.000 con ma, việc quét tuyến tính dễ dàng có giá phải chăng, nhưng việc mô phỏng trực tiếp các trận chiến là không cần thiết vì hoạt động này có đặc tính rất mạnh. Bất kỳ giải pháp nào thử mọi chuỗi chiến đấu có thể sẽ bùng nổ vì số lượng lựa chọn có thể tăng lên nhanh chóng sau mỗi lần loại bỏ. 

Các trường hợp nguy hiểm chính xuất phát từ những tình huống mà cuộc chiến thậm chí không thể bắt đầu. Nếu chỉ có một con ma thì có ngay câu trả lời`YES`bởi vì không ai cần phải bị loại bỏ. Ví dụ:```
Input:
1
7

Output:
YES
```Việc thực hiện bất cẩn mà chỉ tìm kiếm một cuộc chiến có thể xảy ra sẽ trả về sai`NO`. 

Trường hợp quan trọng khác là khi mọi con ma đều có tuổi bằng nhau. Ví dụ:```
Input:
3
5
5
5

Output:
NO
```Không có hai hồn ma liền kề nào có độ tuổi khác nhau nên không có hoạt động nào là hợp pháp. Quá trình dừng lại với ba bóng ma thay vì một. 

Một lỗi phổ biến là chỉ kiểm tra xem có nhiều độ tuổi khác nhau ở đâu đó trong dòng hay không. Sự liền kề quan trọng đối với bước đi đầu tiên. Ví dụ:```
Input:
4
3
3
3
4

Output:
YES
```Hai con ma cuối cùng có thể chiến đấu ngay lập tức. Tuy nhiên, nếu tất cả các giá trị đều bằng nhau thì không tồn tại điểm bắt đầu như vậy. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là mô phỏng quá trình. Chúng ta có thể thử mọi cặp liền kề có thể, loại bỏ bóng ma trẻ hơn, tăng bóng ma cũ hơn và tiếp tục đệ quy cho đến khi dòng có một bóng ma hoặc không thể di chuyển được. Điều này sẽ đúng vì nó khám phá mọi chuỗi lựa chọn có thể có. 

Vấn đề là cây tìm kiếm này rất lớn. Trong trường hợp xấu nhất, nhiều cặp khác nhau có thể được chọn ở mọi giai đoạn. Với 10.000 con ma, thậm chí chỉ xem xét một phần rất nhỏ của tất cả các trình tự có thể xảy ra là không thể. 

Nhận xét quan trọng là trận chiến thành công đầu tiên đã thay đổi hoàn toàn tình thế. Giả sử hai hồn ma liền kề có tuổi khác nhau. Người lớn hơn sẽ thắng và tăng tuổi của mình lên một. Nếu tuổi lớn hơn là`x`, người sống sót trở thành`x + 1`, lớn hơn mọi thời đại tồn tại trước cuộc chiến. 

Điều này tạo ra một con ma mạnh nhất độc nhất. Con ma đó bây giờ có thể đánh bại bất kỳ người hàng xóm nào vì nó luôn già hơn họ. Sau mỗi chiến thắng, nó thậm chí còn trở nên cũ hơn nên nó có thể tiếp tục di chuyển qua hàng cho đến khi những người khác đi hết. 

Vì điều này, chúng ta không cần phải tìm ra trình tự chính xác của các trận đánh. Chúng ta chỉ cần biết liệu trận chiến đầu tiên có tồn tại hay không. Trận chiến đầu tiên diễn ra chính xác khi có một cặp liền kề có độ tuổi khác nhau. 

Lực lượng vũ phu hoạt động vì nó tuân theo tất cả các mệnh lệnh chiến đấu hợp lệ có thể có, nhưng không thành công vì có quá nhiều sự lựa chọn. Nhận xét rằng trận chiến thành công đầu tiên tạo ra người chiến thắng không thể ngăn cản sẽ giảm toàn bộ vấn đề xuống việc kiểm tra những khác biệt liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng ma | Độ sâu đệ quy O(N) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tuổi của tất cả các hồn ma. Nếu chỉ có một con ma, hãy trở về`YES`bởi vì trạng thái cuối cùng cần thiết đã đạt được. 
2. Quét từng cặp ma lân cận và so sánh độ tuổi của chúng. Nếu cặp nào có tuổi khác nhau thì trả về`YES`. 

Cặp liền kề không bằng nhau đầu tiên có thể chiến đấu. Sau trận chiến đó, người sống sót trở nên già hơn mọi hồn ma tồn tại trước đó nên có thể tiêu diệt toàn bộ dòng còn lại. 
3. Nếu quá trình quét kết thúc mà không tìm thấy bất kỳ cặp liền kề không bằng nhau nào, hãy quay lại`NO`. 

Trong tình huống này, mọi con ma đều có độ tuổi như nhau, có nghĩa là không có động thái hợp pháp nào tồn tại ngay từ đầu. 

Tại sao nó hoạt động: Nếu tồn tại một cặp liền kề không bằng nhau, cuộc chiến đầu tiên sẽ tạo ra một con ma có tuổi lớn hơn tất cả các độ tuổi ban đầu. Vì mọi đối thủ sau này đều không già hơn con ma này nên nó luôn có thể giành chiến thắng trong trận chiến tiếp theo và thậm chí còn già hơn. Vì vậy, mọi bóng ma khác cuối cùng đều có thể bị loại bỏ. Nếu không có cặp liền kề không bằng nhau thì tất cả các hồn ma đều có cùng độ tuổi và không thể đánh nhau được nên việc giảm bớt hàng là không thể. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ages = [int(input()) for _ in range(n)]

    if n == 1:
        print("YES")
        return

    for i in range(n - 1):
        if ages[i] != ages[i + 1]:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ xử lý trường hợp bóng ma đơn vì không cần so sánh ở đó. Sau đó, nó chỉ kiểm tra những con ma lân cận, bởi vì nước đi đầu tiên hợp pháp yêu cầu hai con ma liền kề có độ tuổi khác nhau. 

Quá trình quét dừng lại ngay khi tìm thấy trận chiến đầu tiên có thể xảy ra. Không cần phải mô phỏng quá trình còn lại vì hồn ma chiến thắng sau trận chiến đó đảm bảo sẽ chiếm ưu thế trong mọi trận chiến sau này. 

Vòng lặp sử dụng`n - 1`so sánh, do đó ranh giới được chọn cẩn thận để tránh truy cập vào phần tử ở cuối danh sách. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5
5
3
4
4
5
```Quá trình quét hoạt động như sau: 

| Đã kiểm tra chỉ mục | Trái tuổi | Đúng tuổi | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 5 | 3 | Khác nhau, câu trả lời là CÓ | 

Hai con ma đầu tiên có thể chiến đấu. tuổi`5`hồn ma sống sót và trở nên già đi`6`, già hơn mọi hồn ma khác. Dấu vết chứng minh tại sao chỉ có sự tồn tại của một nước đi đầu tiên hợp lệ mới quan trọng. 

Đối với mẫu thứ hai:```
3
1
1
1
```Quá trình quét sẽ kiểm tra mọi cặp lân cận: 

| Đã kiểm tra chỉ mục | Trái tuổi | Đúng tuổi | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | Không đánh nhau | 
| 1 | 1 | 1 | Không đánh nhau | 

Không có động thái hợp pháp nào tồn tại, vì vậy câu trả lời là`NO`. Điều này xác nhận trường hợp cạnh hoàn toàn bằng nhau. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cặp liền kề được kiểm tra một lần. | 
| Không gian | O(N) | Độ tuổi đầu vào được lưu trữ trong một danh sách. | 

Thuật toán chỉ thực hiện một lần vượt qua các bóng ma, dễ dàng phù hợp với giới hạn cho`N = 10000`. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n = int(input())
    ages = [int(input()) for _ in range(n)]

    if n == 1:
        ans = "YES"
    else:
        ans = "NO"
        for i in range(n - 1):
            if ages[i] != ages[i + 1]:
                ans = "YES"
                break

    sys.stdin = old_stdin
    return ans + "\n"

# provided samples
assert solve_input("""5
5
3
4
4
5
""") == "YES\n", "sample 1"

assert solve_input("""3
1
1
1
""") == "NO\n", "sample 2"

# custom cases
assert solve_input("""1
10
""") == "YES\n", "single ghost"

assert solve_input("""5
7
7
7
7
7
""") == "NO\n", "all equal values"

assert solve_input("""4
2
2
2
3
""") == "YES\n", "difference at boundary"

assert solve_input("""6
9
9
8
9
9
9
""") == "YES\n", "middle unequal pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 10`|`YES`| Một dòng đã có sẵn một bản ghost là xong. | 
|`5 / all 7s`|`NO`| Không có động thái đầu tiên hợp pháp tồn tại. | 
|`2 2 2 3`|`YES`| Một cuộc chiến ở cuối dòng là đủ. | 
|`9 9 8 9 9 9`|`YES`| Bất kỳ sự khác biệt liền kề nào sẽ tạo ra người chiến thắng cuối cùng. | 

# Vỏ cạnh 

Khi chỉ có một ghost, thuật toán trả về`YES`ngay lập tức. Đối với đầu vào:```
1
42
```không có trận chiến nào để thực hiện, nhưng dòng đã chứa đúng một con ma còn lại. 

Khi tất cả các hồn ma có độ tuổi giống hệt nhau thì mọi so sánh liền kề đều không thành công. Đối với đầu vào:```
4
6
6
6
6
```quá trình quét kiểm tra ba cặp và mỗi cặp đều có độ tuổi bằng nhau. Vì trận chiến đầu tiên không thể xảy ra nên trạng thái cuối cùng không bao giờ chứa được một bóng ma nên thuật toán trả về`NO`. 

Khi sự khác biệt duy nhất ở gần ranh giới, thuật toán vẫn thành công. Đối với đầu vào:```
4
3
3
3
8
```cặp cuối cùng là không bằng nhau. tuổi`8`ma đánh bại tuổi tác`3`ma và trở thành tuổi`9`, sau đó nó có thể đánh bại từng con ma còn lại. Quá trình quét tìm thấy cặp này và trả về`YES`.
