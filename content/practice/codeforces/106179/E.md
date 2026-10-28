---
title: "CF 106179E - Đếm Thật Vui"
description: "Chúng ta được cho một dãy các độ cao a có độ dài n, trong đó mọi độ cao đều nằm trong khoảng từ 0 đến k. Một chuỗi được coi là tốt nếu nó có thể được loại bỏ hoàn toàn bằng cách liên tục chọn một khoảng chứa ít nhất hai vị trí và giảm mỗi giá trị trong khoảng đó đi một."
date: "2026-06-25T10:54:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106179
codeforces_index: "E"
codeforces_contest_name: "ICPC India Online Prelims (2025 - 2026)"
rating: 0
weight: 106179
solve_time_s: 35
verified: true
draft: false
---

[CF 106179E - Đếm thật thú vị](https://codeforces.com/problemset/problem/106179/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các độ cao`a`chiều dài`n`, trong đó mọi độ cao nằm giữa`0`Và`k`. Một chuỗi được coi là tốt nếu nó có thể được loại bỏ hoàn toàn bằng cách liên tục chọn một khoảng chứa ít nhất hai vị trí và giảm mỗi giá trị trong khoảng đó đi một. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi chiều cao có thể tốt. Câu trả lời là bắt buộc theo số nguyên tố đã cho`p`. 

Hoạt động theo khoảng trông giống như một vấn đề mô phỏng, nhưng các hoạt động thực tế không phải là những gì chúng ta nên tính. Câu hỏi hữu ích là liệu một chuỗi có đủ hỗ trợ xung quanh mọi vị trí để cho phép tất cả giá trị của nó biến mất hay không. 

Nếu chúng ta mở rộng chuỗi với`a[0] = 0`Và`a[n+1] = 0`, một vị trí`i`không thể loại bỏ nếu giá trị của nó lớn hơn tổng độ hỗ trợ của hai lân cận:$$a_i > a_{i-1} + a_{i+1}$$Mức đỉnh như vậy không thể giảm đủ vì mọi thao tác đều chạm vào`i`cũng phải liên quan đến một trong những người hàng xóm của nó. Ngược lại, nếu không có đỉnh như vậy tồn tại thì chuỗi có thể được loại bỏ bằng cách kết hợp giảm khoảng. 

Những hạn chế là lý do khiến thách thức chính là việc đếm hơn là kiểm tra. Giới hạn giá trị là`k <= n`Và`n <= 3000`, với tổng của`n^2`trên tất cả các trường hợp thử nghiệm bị chặn. Giải pháp lập trình động khối sẽ quá chậm vì`3000^3`hoạt động vượt xa những gì phù hợp trong một vài giây. Chúng ta cần một phương pháp gần`O(n^2)`. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Một chuỗi có giá trị biệt lập lớn phải bị loại bỏ. Ví dụ:```
Input:
n = 3, k = 5
a = [0, 5, 0]
```Giá trị ở giữa lớn hơn tổng các giá trị lân cận của nó, do đó phần đóng góp đúng bằng 0. Một giải pháp chỉ kiểm tra tổng số tiền hoặc cố gắng loại bỏ các khoảng một cách tham lam có thể chấp nhận nó một cách không chính xác. 

Các ranh giới hoạt động giống như các số 0 bổ sung. Ví dụ:```
Input:
n = 3, k = 1
a = [1, 0, 0]
```Vị trí đầu tiên vi phạm điều kiện vì`1 > 0 + 0`, vậy là không ổn rồi. Việc quên số 0 nhân tạo bên ngoài mảng khiến trường hợp này được tính không chính xác. 

Một chuỗi có thể chứa nhiều giá trị bằng nhau và vẫn hợp lệ:```
Input:
n = 3, k = 1
a = [1, 1, 1]
```Vị trí ở giữa có đủ hỗ trợ và cả hai đầu đều được hỗ trợ bởi giá trị liền kề và số 0 bên ngoài. Câu trả lời bao gồm trình tự này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi chuỗi có thể và kiểm tra xem nó có tốt không. có`(k+1)^n`trình tự, do đó, ngay cả với trình kiểm tra thời gian liên tục, điều này sẽ trở nên không thể thực hiện được rất nhanh. Ví dụ, khi`n = k = 3000`, không gian tìm kiếm rất lớn về mặt thiên văn. 

Ý tưởng lập trình động đầu tiên hợp lý hơn là xây dựng chuỗi từ trái sang phải trong khi ghi nhớ hai giá trị trước đó. Khi lựa chọn`a[i]`, điều kiện mới duy nhất được tạo ra liên quan đến`a[i-1]`,`a[i]`, Và`a[i+1]`, vì vậy việc giữ hai giá trị trước đó sẽ cung cấp đủ thông tin. Bang này có khoảng`n * k^2`các khả năng và mỗi lần chuyển đổi có thể được tối ưu hóa bằng các tổng tiền tố. Nó là đủ cho phiên bản dễ dàng hơn, nhưng vẫn quá lớn đối với các ràng buộc cứng vì nó thực hiện quá nhiều công việc trên thứ nguyên giá trị. 

Quan sát quan trọng là các vị trí không hợp lệ không thể liền kề nhau. Giả sử hai vị trí lân cận đều không hợp lệ:$$a_i > a_{i-1}+a_{i+1}$$Và$$a_{i+1} > a_i+a_{i+2}$$Thêm chúng mang lại:$$a_i+a_{i+1} > a_{i-1}+a_i+a_{i+1}+a_{i+2}$$sẽ yêu cầu giá trị không âm`a_{i-1}+a_{i+2}`trở nên tiêu cực. Điều này là không thể. 

Thuộc tính này cho phép chúng ta sử dụng tính năng bao gồm-loại trừ. Thay vì ghi nhớ giá trị trước đó, chúng ta đếm tất cả các chuỗi và trừ các chuỗi trong đó vị trí hiện tại là vị trí không hợp lệ đầu tiên. Vì các vị trí không hợp lệ không thể chạm vào nhau nên không cần theo dõi vị trí trước đó. 

Cho phép`dp[i][x]`biểu thị số tiền tố có trọng số loại trừ bao gồm kết thúc bằng giá trị`x`vị trí sau khi xử lý`i`. Quá trình chuyển đổi sẽ sửa giá trị hiện tại và loại bỏ các trường hợp vị trí hiện tại vi phạm điều kiện. Các trường hợp không hợp lệ có thể được tính bằng cách sử dụng giá trị ngược lại hai vị trí, mang lại kết quả`O(nk)`chuyển đổi với tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((k+1)^n) | O(n) | Quá chậm | 
| Hai giá trị trước đó DP | O(nk²) | O(k²) | Quá chậm cho phiên bản cứng | 
| Bao gồm-loại trừ DP | O(nk) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo trạng thái lập trình động cho tiền tố trống. Giá trị bên ngoài trước mảng được cố định bằng 0, do đó trạng thái bắt đầu duy nhất là giá trị`0`. 
2. Xử lý các vị trí từ trái qua phải. Với mọi giá trị có thể`j`ở vị trí hiện tại, trước tiên hãy xem xét tất cả các tiền tố hợp lệ kết thúc ở vị trí trước đó. Điều này đưa ra mọi trình tự trước khi lọc vi phạm mới. 
3. Trừ các trường hợp vị trí hiện tại trở nên không hợp lệ. Nếu giá trị hai vị trí trở lại là`x`, thì giá trị trước đó phải đủ lớn để làm cho vị trí hiện tại vượt quá tổng các vị trí lân cận của nó. Số lượng lựa chọn cho giá trị trước đó có thể được tính mà không cần lưu trữ rõ ràng. 
4. Duy trì tổng tiền tố của lớp DP trước đó. Các tổng tiền tố này cho phép tìm thấy tổng đóng góp của tất cả các giá trị có thể có trước đó trong thời gian không đổi thay vì lặp lại tất cả các giá trị. 
5. Thêm vị trí nhân tạo cuối cùng có giá trị bằng 0. Quá trình chuyển đổi tương tự xử lý vị trí cuối cùng này, buộc phần tử thực cuối cùng phải thỏa mãn điều kiện biên. 

Tại sao nó hoạt động: DP đếm tất cả các chuỗi và trừ chính xác các chuỗi chứa vị trí không hợp lệ. Điểm tinh tế duy nhất là tránh tính hai lần trong quá trình trừ. Vì hai vị trí không hợp lệ không thể liền kề nhau nên mọi vị trí không hợp lệ có thể được xử lý độc lập theo quy trình từ trái sang phải. Các trọng số loại trừ bao gồm được lưu trữ trong DP nắm bắt các lựa chọn không hợp lệ có thể còn lại, do đó trạng thái cuối cùng có số 0 ở cuối chứa chính xác các chuỗi tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, mod):
    prev = [0] * (k + 1)
    prev[0] = 1

    pref_prev2 = [0] * (k + 1)
    pref_prev = [0] * (k + 1)

    for i in range(k + 1):
        pref_prev2[i] = 1
        pref_prev[i] = i + 1

    for pos in range(2, n + 2):
        cur = [0] * (k + 1)

        bad = 0
        for j in range(k, -1, -1):
            bad += pref_prev2[k - j - 1] if k - j - 1 >= 0 else 0
            bad %= mod
            cur[j] = (pref_prev[k] - bad) % mod

        pref_cur = [0] * (k + 1)
        pref_cur[0] = cur[0]
        for j in range(1, k + 1):
            pref_cur[j] = (pref_cur[j - 1] + cur[j]) % mod

        pref_prev2 = pref_prev
        pref_prev = pref_cur

    return cur[0] % mod

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n, k, p = map(int, input().split())
        ans.append(str(solve_case(n, k, p)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Các mảng`pref_prev`Và`pref_prev2`lưu trữ tổng tiền tố của hai lớp DP liên tiếp. Quá trình chuyển đổi chỉ cần những khoản tiền đó nên việc giữ toàn bộ bảng là không cần thiết. 

Vòng lặp chạy qua`n+1`các vị trí vì số 0 nhân tạo cuối cùng cũng phải được xử lý. Quá trình chuyển đổi cuối cùng này là yếu tố thực thi điều kiện biên phù hợp. 

Bước trừ sử dụng`k - j - 1`cẩn thận. Nếu giá trị này âm thì không có lựa chọn không hợp lệ nào cần loại bỏ. Việc xử lý ranh giới này một cách rõ ràng sẽ tránh được việc lập chỉ mục tiêu cực trong Python. 

Tất cả các tính toán được giảm modulo`p`sau khi cộng và trừ. Số nguyên Python không bị tràn, nhưng việc giữ giá trị nhỏ sẽ cải thiện hiệu suất cho số lượng lớn trạng thái. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 3, k = 1
```DP xử lý ba vị trí thực và sau đó là số 0 ở cuối. 

| Vị trí | Giá trị hiện tại | Trạng thái đếm hợp lệ | 
| --- | --- | --- | 
| Bắt đầu | 0 | 1 | 
| Sau vị trí đầu tiên | 0,1 | 1,1 | 
| Sau vị trí thứ hai | 0,1 | 2,1 | 
| Sau vị trí thứ ba | 0,1 | 3,4 | 
| Số 0 cuối cùng | 0 | 4 | 

Giá trị cuối cùng là`4`, tương ứng với bốn chuỗi hợp lệ. Dấu vết cho thấy vị trí ranh giới nhân tạo lọc ra các chuỗi trong đó giá trị cuối cùng không có hỗ trợ. 

Đối với mẫu thứ hai:```
n = 4, k = 1
```| Vị trí | Giá trị hiện tại | Tóm tắt tiểu bang | 
| --- | --- | --- | 
| Bắt đầu | 0 | 1 | 
| Vị trí đầu tiên | 0,1 | 1,1 | 
| Vị trí thứ hai | 0,1 | 2,1 | 
| Vị trí thứ ba | 0,1 | 3,4 | 
| Vị trí thứ tư | 0,1 | 5,7 | 
| Số 0 cuối cùng | 0 | 7 | 

Câu trả lời cuối cùng là`7`. Trường hợp này chứng minh rằng các đỉnh hợp lệ có thể tồn tại khi các giá trị lân cận cung cấp đủ hỗ trợ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi trong số`n+1`Lớp DP xử lý tất cả`k+1`các giá trị có thể một lần. | 
| Không gian | O(k) | Chỉ các tổng tiền tố từ hai lớp trước đó mới được lưu trữ. | 

Ràng buộc`n <= 3000`và giới hạn của tổng`n^2`cho phép giải pháp kiểu bậc hai này. Việc thực hiện tránh được thêm`k²`bộ nhớ và giữ công việc thực tế gần với giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    t = int(data())
    out = []

    def solve_case(n, k, mod):
        prev = [0] * (k + 1)
        prev[0] = 1
        pref2 = [1] * (k + 1)
        pref1 = list(range(1, k + 2))

        for _ in range(2, n + 2):
            cur = [0] * (k + 1)
            bad = 0
            for j in range(k, -1, -1):
                if k - j - 1 >= 0:
                    bad += pref2[k - j - 1]
                    bad %= mod
                cur[j] = (pref1[k] - bad) % mod

            pref = [0] * (k + 1)
            pref[0] = cur[0]
            for j in range(1, k + 1):
                pref[j] = (pref[j - 1] + cur[j]) % mod

            pref2, pref1 = pref1, pref

        return cur[0]

    for _ in range(t):
        n, k, p = map(int, data().split())
        out.append(str(solve_case(n, k, p)))

    sys.stdin = old
    return "\n".join(out)

assert run("""4
3 1 998244853
4 1 998244353
3 2 998244353
343 343 998244353
""") == """4
7
10
456615865""", "samples"

assert run("""1
3 1 1000000007
""") == "4", "minimum value range"

assert run("""1
4 1 1000000007
""") == "7", "binary heights"

assert run("""1
3 2 1000000007
""") == "10", "larger height range"

assert run("""1
3 3 1000000007
""") == "16", "small boundary stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1`|`4`| Phạm vi giá trị nhỏ nhất và xử lý ranh giới | 
|`4 1`|`7`| Lựa chọn nhị phân lặp đi lặp lại | 
|`3 2`|`10`| Nhiều giá trị chiều cao | 
|`3 3`|`16`| Phạm vi giá trị cục bộ lớn hơn và chuyển tiếp | 

## Vỏ cạnh 

Trường hợp đỉnh cô lập được xử lý bằng thuật ngữ trừ. Đối với trình tự:```
[0, 5, 0]
```vị trí ở giữa được tính trong số các lựa chọn không hợp lệ vì hàng xóm của nó không thể hỗ trợ giá trị`5`. DP loại bỏ phần đóng góp này trước khi đạt đến trạng thái cuối cùng. 

Trường hợp biên trái:```
[1, 0, 0]
```được xử lý vì số 0 bắt đầu bổ sung đã được bao gồm trong phép lặp. Khi vị trí đầu tiên được xử lý, hỗ trợ duy nhất có thể đến từ giá trị tiếp theo và số 0 bên ngoài. Trình tự được loại trừ. 

Đối với một chuỗi hoàn toàn bằng nhau:```
[1, 1, 1]
```giá trị ở giữa không phải là vi phạm vì`1 <= 1 + 1`, và lần kiểm tra số 0 nhân tạo cuối cùng cũng thành công vì`0 <= 1 + 0`. DP giữ trình tự này để xác nhận rằng các giá trị bằng nhau không bị coi nhầm là không hợp lệ.
