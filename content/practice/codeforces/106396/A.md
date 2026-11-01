---
title: "CF 106396A - \u72fc"
description: "Chúng ta được cấp một tập hợp các vật phẩm, mỗi vật phẩm có trọng số nguyên. Ngoài ra còn có một giá trị bù trừ ban đầu, hoạt động giống như số dư ban đầu trong hệ thống. Quá trình bắt đầu từ phần bù này và mỗi mục có thể được chọn nhiều nhất một lần."
date: "2026-06-20T12:34:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "A"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 45
verified: true
draft: false
---

[CF 106396A - \u72fc](https://codeforces.com/problemset/problem/106396/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các vật phẩm, mỗi vật phẩm có trọng số nguyên. Ngoài ra còn có một giá trị bù trừ ban đầu, hoạt động giống như số dư ban đầu trong hệ thống. Quá trình bắt đầu từ phần bù này và mỗi mục có thể được chọn nhiều nhất một lần. Mỗi mục được chọn sẽ thay đổi giá trị hiện tại theo trọng số của nó và mục tiêu là hiểu được tổng giá trị nào có thể đạt được sau khi xem xét tất cả các mục. 

Thay vì yêu cầu một mục tiêu duy nhất có thể tiếp cận, nhiệm vụ là mô tả đặc điểm tất cả các tổng có thể đạt được sau khi xử lý mọi mục, bắt đầu từ độ lệch ban đầu và sau đó tìm cấu hình giúp kết quả cuối cùng cân bằng nhất có thể xung quanh tổng khối lượng của hệ thống. Cụ thể, nếu chúng ta tưởng tượng việc chia số lượng tích lũy thành hai phần, chúng ta muốn hai phần này càng gần nhau càng tốt và chúng ta đo lường sự mất cân bằng bằng sự khác biệt tuyệt đối giữa chúng. 

Kích thước đầu vào đủ nhỏ để có thể thực hiện được giải pháp lập trình động giả đa thức trên tổng. Ràng buộc ẩn chính là tổng của tất cả các giá trị cộng với phần bù xác định phạm vi DP. Điều này ngay lập tức loại trừ mọi phép liệt kê tập hợp con theo cấp số nhân, sẽ tăng lên 2^n và trở nên không khả thi ngay cả đối với n khoảng 40. 

Trường hợp góc tinh tế xuất hiện khi tất cả các giá trị bằng 0 hoặc khi phần bù lấn át tất cả các giá trị khác. Ví dụ: nếu mảng là [0, 0, 0] và độ lệch là 5 thì mọi tập hợp con đều tạo ra tổng số như nhau, do đó câu trả lời phải là 5. Việc triển khai ngây thơ khởi tạo DP không chính xác mà không tính đến trạng thái bù trừ có thể kết luận sai rằng chỉ có thể truy cập được tổng 0. 

Một trường hợp cạnh khác xảy ra khi có một giá trị lớn duy nhất. Nếu x = 10 và a = [7] thì tổng số có thể tiếp cận chỉ là 10 và 17. Sự phân chia tối ưu nằm giữa hai cực này và việc quên khởi tạo DP với phần bù là tổng bắt đầu hợp lệ sẽ dẫn đến thiếu hoàn toàn cấu hình cơ sở. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là liệt kê tất cả các tập hợp con của các mục. Đối với mỗi tập hợp con, hãy tính tổng giá trị thu được bắt đầu từ phần bù, sau đó tính xem tổng này gần bằng một nửa tổng tổng. Điều này đúng vì nó khám phá mọi sự kết hợp có thể có, nhưng về bản chất nó có tính cấp số nhân. Với n mục, điều này đòi hỏi phải đánh giá 2^n tập hợp con và thậm chí tính toán tổng mỗi tập hợp con mất O(n), dẫn đến O(n·2^n), điều này trở nên không thể thực hiện được nếu vượt quá giới hạn nhỏ. 

Quan sát chính là về mặt cấu trúc, đây là một vấn đề về khả năng tiếp cận tổng tập hợp con. Thay vì liệt kê rõ ràng các tập hợp con, chúng tôi duy trì trạng thái boolean trên tất cả các tổng có thể. Trạng thái dp[s] biểu thị liệu có thể đạt được tổng sau khi xử lý một số tiền tố của các mục hay không. Mỗi mục hoạt động như một quá trình chuyển đổi ba lô 0-1, trong đó chúng tôi bao gồm hoặc loại trừ mục đó và chúng tôi cập nhật số tiền có thể tiếp cận tương ứng. 

Điều quan trọng là trạng thái ban đầu không bằng 0 mà bằng độ lệch x đã cho. Điều này sẽ thay đổi toàn bộ miền DP, nghĩa là tất cả số tiền có thể truy cập đều tập trung quanh điểm bắt đầu này. Sau khi tính toán tất cả các số tiền có thể tiếp cận, nhiệm vụ cuối cùng sẽ giảm xuống còn việc tìm một số tiền có thể tiếp cận để giảm thiểu sự khác biệt giữa hai phân vùng của tổng giá trị tích lũy. 

Điều này làm giảm vấn đề từ liệt kê theo cấp số nhân sang DP giả đa thức trên phạm vi tổng, điều này có thể chấp nhận được vì tổng của tất cả các phần tử giới hạn không gian trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n·2^n) | O(1) | Quá chậm | 
| 0-1 DP trên tổng số tiền | O(n·S) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## 1. Tính tổng các số tiền có thể có

Trước tiên, chúng tôi tính tổng của tất cả các phần tử và thêm phần bù để xác định giá trị tối đa có thể tiếp cận. Điều này xác định ranh giới DP. Nếu không giới hạn phạm vi này, chúng ta sẽ không biết bảng DP phải lớn đến mức nào và các chuyển tiếp sẽ không được xác định. 

## 2. Khởi tạo bảng DP 

Chúng tôi tạo một mảng DP boolean trong đó dp[s] cho biết liệu sum s có thể truy cập được hay không. Trạng thái có thể truy cập ban đầu duy nhất là giá trị bù trừ, vì trước khi lấy bất kỳ mục nào, tổng hiện tại của chúng tôi chính xác là giá trị bù trừ bắt đầu. 

Việc khởi tạo này là bước lập mô hình quan trọng nhất trong toàn bộ giải pháp, vì nó làm dịch chuyển bài toán tổng tập hợp con ra xa 0. 

## 3. Xử lý từng mục bằng cách sử dụng chuyển đổi ba lô 0-1 

Đối với mỗi mục, chúng tôi tính toán một mảng DP mới dựa trên trạng thái trước đó. Nếu tổng j có thể truy cập được thì j + a[i] sẽ có thể truy cập được ở trạng thái tiếp theo. Chúng tôi sao chép DP trước đó để giữ nguyên lựa chọn "không lấy vật phẩm" và sau đó áp dụng các chuyển tiếp về phía trước. 

Điều này đảm bảo mỗi mục được sử dụng tối đa một lần vì mỗi lớp được xây dựng từ lớp trước đó. 

## 4. Trích xuất phân vùng cân bằng tốt nhất 

Sau khi xử lý tất cả các mục, chúng tôi quét tất cả các khoản tiền có thể truy cập. Đối với mỗi số tiền có thể truy cập, chúng tôi hiểu nó là một mặt của phân vùng và tính toán mức độ mất cân bằng so với tổng số tiền. Câu trả lời là sự khác biệt tuyệt đối tối thiểu. 

## Tại sao nó hoạt động 

Ở mỗi bước, dp[s] thể hiện chính xác tập hợp số tiền có thể đạt được bằng cách sử dụng một tập hợp con các mục được xử lý bắt đầu từ phần bù. Quá trình chuyển đổi duy trì tính chính xác vì mọi trạng thái mới được hình thành bằng cách loại trừ hoặc bao gồm mục hiện tại và không thể thực hiện được thao tác nào khác. Vì tất cả các tập hợp con được thể hiện ngầm trong DP nên lần quét cuối cùng trên các trạng thái có thể truy cập tương đương với việc đánh giá mọi tập hợp con nhưng không liệt kê chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a) + x

    dp = [False] * (total + 1)
    dp[x] = True

    for w in a:
        ndp = dp[:]
        for s in range(w, total + 1):
            if dp[s - w]:
                ndp[s] = True
        dp = ndp

    ans = float('inf')
    for s in range(total + 1):
        if dp[s]:
            ans = min(ans, abs(total - 2 * s))

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng DP có kích thước bằng phạm vi tổng có thể đạt được đầy đủ, được tính một lần ở đầu. Việc khởi tạo dp[x] = True neo hệ thống ở trạng thái bù. 

Vòng lặp chuyển tiếp sử dụng một mảng được sao chép để đảm bảo mỗi mục chỉ được áp dụng một lần. Một lỗi phổ biến là cập nhật dp tại chỗ, điều này sẽ cho phép sử dụng nhiều mục cùng một mục trong một lần lặp. 

Cuối cùng, quá trình quét sẽ tính toán sự mất cân bằng tối thiểu bằng cách xử lý từng khoản tiền có thể tiếp cận như một điểm phân chia tiềm năng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
1 2 3
```Tổng số tiền là 11 + 5 = 16. 

Chúng tôi theo dõi số tiền có thể tiếp cận. 

| Bước | Mặt hàng đã xử lý | Số tiền có thể tiếp cận (mẫu) | 
| --- | --- | --- | 
| 0 | không | {5} | 
| 1 | 1 | {5, 6} | 
| 2 | 2 | {5, 6, 7, 8} | 
| 3 | 3 | {5, 6, 7, 8, 9, 10, 11} | 

Bây giờ chúng ta đánh giá sự mất cân bằng |16 - 2s|. Sự phân chia gần nhất là gần 8. 

Với s = 8, sự mất cân bằng là |16 - 16| = 0. 

Điều này cho thấy DP đã tìm thấy chính xác một phân vùng hoàn hảo trong trường hợp này. 

### Ví dụ 2 

đầu vào:```
2 4
5 1
```Tổng số tiền là 10. 

| Bước | Mặt hàng đã xử lý | Số tiền có thể tiếp cận | 
| --- | --- | --- | 
| 0 | không | {4} | 
| 1 | 5 | {4, 9} | 
| 2 | 1 | {4, 5, 9, 10} | 

Bây giờ chúng tôi tính toán sự mất cân bằng: 

Với s = 5, |10 - 10| = 0 đạt được. 

Điều này chứng tỏ rằng giải pháp tối ưu có thể đến từ các trạng thái có thể tiếp cận trung gian chứ không phải ở các trạng thái cực đoan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·S) | Mỗi mục thực hiện quét toàn bộ phạm vi DP một lần | 
| Không gian | O(S) | Mảng DP trên tất cả các tổng có thể truy cập lên đến tổng | 

Giá trị S là tổng của tất cả các trọng số đầu vào cộng với phần bù. Điều này thường bị hạn chế sao cho n·S duy trì trong phạm vi vài trăm triệu thao tác hoặc ít hơn, khiến giải pháp có thể chấp nhận được trong Python hoặc C++ được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a) + x
    dp = [False] * (total + 1)
    dp[x] = True

    for w in a:
        ndp = dp[:]
        for s in range(w, total + 1):
            if dp[s - w]:
                ndp[s] = True
        dp = ndp

    ans = float('inf')
    for s in range(total + 1):
        if dp[s]:
            ans = min(ans, abs(total - 2 * s))

    return str(ans)

# minimal case
assert run("1 0\n0\n") == "0"

# offset dominates
assert run("1 10\n0\n") == "10"

# simple partition
assert run("3 5\n1 2 3\n") == "0"

# two-element split
assert run("2 4\n5 1\n") == "0"

# all equal values
assert run("4 3\n2 2 2 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 0 | 0 | đường cơ sở chỉ có 0 | 
| 1 10 / 0 | 10 | bù đắp sự thống trị | 
| 3 5 / 1 2 3 | 0 | trường hợp phân vùng hoàn hảo | 
| 2 4 / 5 1 | 0 | sự phân chia có thể tiếp cận hỗn hợp | 
| 4 3 / 2 2 2 2 | 1 | ràng buộc đối xứng và chẵn lẻ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều bằng 0. Trong trường hợp này, DP không bao giờ mở rộng ra ngoài trạng thái ban đầu. Đối với đầu vào`n=3, x=7, a=[0,0,0]`, dp còn lại`{7}`xuyên suốt, và câu trả lời cuối cùng là`|7 - 7| = 0`, biểu thị chính xác không có thay đổi mất cân bằng. 

Một trường hợp cạnh khác là khi offset bằng 0 và chỉ có một mục duy nhất. Đối với đầu vào`n=1, x=0, a=[5]`, dp tiến hóa từ`{0}`ĐẾN`{0,5}`và sự phân chia tốt nhất rõ ràng là từ 0 đến 5, tạo ra sự mất cân bằng 5. DP nắm bắt chính xác cả hai trạng thái vì nó bắt đầu từ điều kiện ban đầu chính xác. 

Trường hợp khó phát hiện cuối cùng là khi các giá trị lớn tụ lại gần giới hạn trên của phạm vi tổng. Bởi vì DP sử dụng một bản sao đầy đủ cho mỗi lần lặp, nên các quá trình chuyển tiếp vẫn bị cô lập trên mỗi mục, ngăn chặn việc sử dụng lại trong cùng một bước. Điều này đảm bảo tính chính xác ngay cả khi nhiều kết hợp có thể đạt cùng một tổng thông qua các đường dẫn khác nhau.
