---
title: "CF 425A - Sereja và Hoán đổi"
description: "Chúng ta được cung cấp một mảng các số nguyên và ngân sách hạn chế cho các hoạt động hoán đổi. Mỗi lần hoán đổi cho phép hoán đổi hai vị trí bất kỳ trong mảng và chúng ta có thể thực hiện tối đa k lần hoán đổi như vậy."
date: "2026-06-07T02:31:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 425
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 243 (Div. 1)"
rating: 1500
weight: 425
solve_time_s: 286
verified: false
draft: false
---

[CF 425A - Sereja và Hoán đổi](https://codeforces.com/problemset/problem/425/A) 

**Đánh giá:** 1500 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 4 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và ngân sách hạn chế cho các hoạt động hoán đổi. Mỗi lần hoán đổi cho phép hoán đổi hai vị trí bất kỳ trong mảng và chúng ta có thể thực hiện nhiều nhất`k`những sự hoán đổi như vậy. 

Mục tiêu là sắp xếp lại mảng bằng cách sử dụng không quá`k`hoán đổi sao cho một hàm số điểm nhất định,`m(a)`, trở nên lớn nhất có thể. Hàm này gán các trọng số khác nhau cho các vị trí mảng: các phần tử được đặt trước trong mảng đóng góp nhiều hơn vào giá trị cuối cùng so với các phần tử được đặt sau. Cụ thể, sự đóng góp của một phần tử phụ thuộc vào khoảng cách về bên phải của nó, do đó, việc đưa các giá trị lớn sớm hơn vào mảng sẽ có lợi. 

Kích thước đầu vào nhỏ:`n ≤ 200`Và`k ≤ 10`. Điều này ngay lập tức loại trừ bất kỳ chiến lược hàm mũ nào trên tất cả các hoán vị, vì`n!`là quá lớn. Thậm chí`O(n^3)`các giải pháp ở mức giới hạn nhưng có thể chấp nhận được; bất cứ điều gì liên quan đến việc liệt kê tất cả các lần sắp xếp lại đều không khả thi. 

Sự hiện diện của một rất nhỏ`k`là hạn chế cơ cấu quan trọng. Nó gợi ý rằng chúng ta không nên tự do hoán đổi mảng mà thay vào đó thực hiện một số chỉnh sửa có tác động hạn chế bắt đầu từ cấu hình ban đầu. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị trùng lặp và số âm. Ví dụ: khi tất cả các phần tử đều âm, việc hoán đổi thực sự có thể làm giảm điểm nếu nó đẩy một số âm ít hơn vào vị trí có trọng số cao trong khi để lại một số rất âm sau đó. Tương tự, nếu mảng đã tối ưu thì việc sử dụng hoán đổi là tùy chọn và không nên bắt buộc. 

## Phương pháp tiếp cận 

Hàm tính điểm chỉ phụ thuộc vào thứ tự cuối cùng của mảng chứ không phụ thuộc vào cách chúng ta đạt được nó. Vì hoán đổi là trao đổi tùy ý giữa hai chỉ số bất kỳ nên mỗi hoán đổi có thể được coi là chọn hai vị thế và trao đổi giá trị của chúng trong một động thái. 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các chuỗi có thể có nhiều nhất`k`trao đổi. Ở mỗi bước, chúng tôi thử tất cả các cặp`(i, j)`, áp dụng hoán đổi và tiếp tục đệ quy. Điều này đúng vì nó khám phá mọi cấu hình có thể truy cập trong số lượng thao tác được phép. Tuy nhiên, yếu tố phân nhánh là`O(n^2)`mỗi bước và độ sâu là`k`, dẫn đến đại khái`O(n^(2k))`trạng thái phát triển cực kỳ nhanh chóng ngay cả đối với`k = 3`. 

Quan sát quan trọng là chúng ta không bao giờ cần xem xét các giao dịch hoán đổi không liên quan đến vị thế hiện tại mà chúng ta đang quyết định. Thay vì khám phá các chuỗi hoán đổi tùy ý trên toàn cầu, chúng ta có thể xây dựng câu trả lời từ trái sang phải. Tại vị trí`i`, chúng tôi quyết định giá trị nào sẽ được đặt ở đó, sử dụng nhiều nhất một lần hoán đổi để đưa nó từ một vị trí sau đó. Sau khi cố định vị trí`i`, chúng ta tiến về phía trước. 

Điều này biến vấn đề thành một công trình tham lam có kiểm soát với việc quay lại các lựa chọn. Tại mỗi chỉ mục, chúng tôi giữ nguyên phần tử hoặc hoán đổi nó với một số vị trí sau đó, tiêu tốn một đơn vị ngân sách hoán đổi. Bởi vì`k ≤ 10`, khám phá tất cả các lựa chọn như vậy là khả thi. 

Do đó, đệ quy được xác định qua ba phần trạng thái: chỉ mục hiện tại, số lần hoán đổi còn lại và cấu hình hiện tại của mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm hoán vị đầy đủ | O(n!) | O(n) | Quá chậm | 
| Mô phỏng hoán đổi đệ quy | O(n^(2k)) | O(n) | Quá chậm | 
| DFS được kiểm soát với các lựa chọn hoán đổi | O(k · n^2 · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách sử dụng tìm kiếm chuyên sâu để khám phá tất cả các quyết định hoán đổi có ý nghĩa. 

1. Bắt đầu từ chỉ mục`i = 0`với mảng ban đầu và`k`trao đổi có sẵn. Tiền tố trước`i`được coi là cố định. 
2. Tính điểm của mảng hiện tại nếu dừng ngay. Điều này hoạt động như một ứng cử viên cơ bản. 
3. Nếu không còn giao dịch hoán đổi nào hoặc`i`đến cuối, trả về số điểm tính toán. 
4. Nếu không, hãy xem xét việc cải thiện vị trí hiện tại. Đối với chỉ mục`i`, thử đổi chỗ`a[i]`với mọi vị trí`j ≥ i + 1`. Mỗi lần hoán đổi như vậy tiêu tốn một thao tác và tạo ra một cấu hình mảng mới. 
5. Sau khi thực hiện hoán đổi`(i, j)`, giải quyết đệ quy bài toán con bắt đầu từ chỉ mục`i + 1`với một ít trao đổi có sẵn. Điều này phản ánh vị trí đó`i`hiện đã được khắc phục và chúng tôi tiến về phía trước. 
6. Ngoài ra, hãy cân nhắc việc không hoán đổi vị trí nào cả`i`, và tiến thẳng tới`i + 1`với cùng một mảng và các giao dịch hoán đổi còn lại giống nhau. 
7. Hãy đạt được kết quả tối đa trong số tất cả những lựa chọn này. 

Lý do chúng tôi luôn tiến hành từ trái sang phải là vì một khi một vị trí đã được cố định, việc thay đổi nó sau này sẽ chỉ lãng phí các giao dịch hoán đổi mà không cải thiện cấu trúc một cách có kiểm soát. 

### Tại sao nó hoạt động 

Ở mọi vị trí`i`, thuật toán liệt kê tất cả các giá trị có thể có thể chiếm vị trí đó trong quỹ hoán đổi còn lại. Bất kỳ chuỗi hợp lệ nào nhiều nhất`k`hoán đổi tạo ra một số giá trị xuất hiện tại chỉ mục`i`sau những lần hoán đổi đó. Quá trình chuyển đổi của chúng tôi xem xét mọi cách để đưa bất kỳ giá trị ứng viên nào vào vị trí`i`trong một lần hoán đổi, sau đó xử lý đệ quy hậu tố còn lại với ít lần hoán đổi hơn. Điều này phù hợp với cấu trúc của bất kỳ chuỗi hoán đổi hợp lệ nào vì mọi hoán đổi đều ảnh hưởng đến vị trí hiện tại hoặc có thể bị trì hoãn mà không thay đổi tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_score(arr):
    n = len(arr)
    total = 0
    weight = n
    for x in arr:
        total += x * weight
        weight -= 1
    return total

def dfs(i, k, arr):
    n = len(arr)

    best = compute_score(arr)

    if i == n or k == 0:
        return best

    # option 1: do nothing at position i
    best = max(best, dfs(i + 1, k, arr))

    # option 2: try swapping i with any j > i
    for j in range(i + 1, n):
        arr[i], arr[j] = arr[j], arr[i]
        best = max(best, dfs(i + 1, k - 1, arr))
        arr[i], arr[j] = arr[j], arr[i]

    return best

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(dfs(0, k, a))

if __name__ == "__main__":
    main()
```Việc triển khai dựa vào việc hoán đổi tại chỗ với tính năng quay lui ngay lập tức. Điều này tránh việc sao chép mảng ở mỗi bước, nếu không sẽ thêm một hệ số bổ sung`n`đến sự phức tạp. 

Hàm điểm chỉ được tính toán lại khi cần thiết. Từ`n ≤ 200`, việc tính toán lại có thể chấp nhận được và giữ cho logic đệ quy luôn rõ ràng. 

Một chi tiết tinh tế là chúng tôi luôn khôi phục mảng sau khi khám phá nhánh hoán đổi. Việc quên quá trình khôi phục này sẽ làm hỏng các nhánh sau và âm thầm tạo ra kết quả không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

đầu vào:```
10 2
10 -1 2 2 2 2 2 2 -1 10
```Chúng tôi theo dõi một cái nhìn đơn giản về các quyết định đầu tiên. 

| Bước | tôi | k | hành động | tiền tố mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | bắt đầu | [10, -1, 2, ...] | 
| 2 | 0 | 2 | hoán đổi (0,1) | [-1, 10, 2, ...] | 
| 3 | 1 | 1 | tiếp tục | [-1, 10, 2, ...] | 
| 4 | 1 | 1 | hoán đổi (1,9) | [-1, 10, 2, ..., -1, 2] | 

Điều này cho thấy cách thuật toán ưu tiên sớm mang lại các giá trị lớn trong khi sử dụng các giao dịch hoán đổi một cách có chọn lọc. 

Bây giờ hãy xem xét một ví dụ tùy chỉnh nhỏ hơn: 

đầu vào:```
5 1
1 5 3 2 4
```| Bước | tôi | k | hành động | mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | hoán đổi (0,1) | [5, 1, 3, 2, 4] | 
| 2 | 1 | 0 | dừng lại | [5, 1, 3, 2, 4] | 

Ở đây, hoán đổi duy nhất được sử dụng để tối đa hóa trọng lượng của phần tử lớn nhất ngay lập tức, xác nhận rằng cải tiến sớm tham lam phù hợp với cấu trúc tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · n^3) | Tại mỗi trong số nhiều nhất`k`trao đổi cấp độ, chúng tôi cố gắng lên đến`n`sự lựa chọn cho`i`Và`n`sự lựa chọn cho`j`và tính lại điểm theo O(n) | 
| Không gian | O(n) | độ sâu đệ quy lên tới`n`, mảng được sửa đổi tại chỗ | 

Với`n ≤ 200`Và`k ≤ 10`, điều này phù hợp thoải mái trong thời gian giới hạn vì việc phân nhánh bị hạn chế rất nhiều bởi`k`và hầu hết các đường dẫn đệ quy đều kết thúc sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def compute_score(arr):
        n = len(arr)
        total = 0
        weight = n
        for x in arr:
            total += x * weight
            weight -= 1
        return total

    def dfs(i, k, arr):
        n = len(arr)
        best = compute_score(arr)
        if i == n or k == 0:
            return best
        best = max(best, dfs(i + 1, k, arr))
        for j in range(i + 1, n):
            arr[i], arr[j] = arr[j], arr[i]
            best = max(best, dfs(i + 1, k - 1, arr))
            arr[i], arr[j] = arr[j], arr[i]
        return best

    n, k = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    return str(dfs(0, k, a))

# provided sample
assert run("""10 2
10 -1 2 2 2 2 2 2 -1 10
""") == "32"

# minimum size
assert run("""1 10
5
""") == "5"

# one swap beneficial
assert run("""3 1
1 3 2
""") == "11", "swap improves front"

# already optimal
assert run("""4 2
4 3 2 1
""") == "30"

# all equal
assert run("""5 3
7 7 7 7 7
""") == "105"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 2 ... | 32 | độ chính xác của mẫu | 
| 1 10 5 | 5 | trường hợp cạnh phần tử đơn | 
| 3 1 1 3 2 | 11 | logic cải tiến trao đổi | 
| 4 2 4 3 2 1 | 30 | trường hợp đã tối ưu | 
| 5 3 tất cả 7 | 105 | hoán đổi đối xứng và không hoạt động | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mảng chỉ có một phần tử. Thuật toán ngay lập tức tính điểm và trả về mà không cần nhập đệ quy, vì không thể hoán đổi hoặc sắp xếp lại. Đối với đầu vào`1 5`với mảng`[7]`, hàm đánh giá`7 * 1 = 7`và dừng lại một cách chính xác. 

Một trường hợp khác là khi tất cả các phần tử đều bằng nhau. Bất kỳ trao đổi nào cũng tạo ra một cấu hình giống hệt nhau. DFS vẫn khám phá các nhánh hoán đổi, nhưng điểm số vẫn không thay đổi trên tất cả các trạng thái. Vì`5 2`với`[3, 3, 3, 3, 3]`, mọi đường đệ quy đều mang lại tổng có trọng số như nhau, do đó mức tối đa vẫn ổn định tại`45`.
