---
title: "CF 105160A - \u6211\u662f\u7ec4\u9898\u4eba"
description: "Chúng ta được cung cấp một danh sách các khó khăn của bài toán, trong đó mỗi bài toán cũng có một mã định danh ngầm được đưa ra bởi vị trí của nó trong đầu vào. Nhiệm vụ là sắp xếp lại các chỉ số bài toán theo độ khó từ nhỏ nhất đến lớn nhất."
date: "2026-06-27T11:00:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "A"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 51
verified: true
draft: false
---

[CF 105160A - \u6211\u662f\u7ec4\u9898\u4eba](https://codeforces.com/problemset/problem/105160/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các khó khăn của bài toán, trong đó mỗi bài toán cũng có một mã định danh ngầm được đưa ra bởi vị trí của nó trong đầu vào. Nhiệm vụ là sắp xếp lại các chỉ số bài toán theo độ khó từ nhỏ nhất đến lớn nhất. Khi hai bài toán có cùng độ khó thì bài có chỉ số gốc nhỏ hơn phải xuất hiện trước. 

Đầu ra không phải là những khó khăn được sắp xếp mà là chuỗi các chỉ số ban đầu sau khi sắp xếp theo quy tắc này. 

Kích thước đầu vào nhỏ, tối đa 1000 vấn đề. Điều này ngay lập tức cho phép mọi cách tiếp cận xung quanh thời gian bậc hai hoặc tốt hơn. Một giải pháp thực hiện theo thứ tự một triệu thao tác cơ bản là hoàn toàn an toàn, trong khi mọi thao tác liên quan đến việc quét toàn bộ lặp đi lặp lại bên trong một vòng lặp khác vẫn có khả năng vượt qua nhưng không cần thiết. 

Một lỗi phổ biến là chỉ sắp xếp các giá trị độ khó mà không theo dõi các chỉ số, làm mất kết quả đầu ra cần thiết. Một sai lầm nữa là sắp xếp các chỉ số mà quên quy tắc tie-break, dẫn đến sắp xếp sai thứ tự khi xuất hiện các giá trị bằng nhau. Ví dụ: nếu đầu vào là`3 1 1`, đầu ra đúng là`2 3 1`, bởi vì hai`1`s giữ trật tự ban đầu của họ. 

Một trường hợp cạnh tinh vi khác xuất hiện khi tất cả các giá trị đều bằng nhau. Nếu như`a = [5, 5, 5]`, đầu ra phải là`1 2 3`, giữ nguyên toàn bộ thứ tự ban đầu. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là liên tục tìm kiếm độ khó nhỏ nhất còn lại, đánh dấu nó là đã sử dụng và thêm chỉ mục của nó vào kết quả. Mỗi lựa chọn yêu cầu quét toàn bộ mảng để tìm mức tối thiểu tiếp theo chưa được sử dụng. Về cơ bản, đây là một quá trình lựa chọn được lặp lại n lần và mỗi lần quét tốn O(n), dẫn đến tổng số thao tác là O(n2). 

Cách tiếp cận này đúng vì nó tuân theo quy tắc sắp xếp: luôn chọn phần tử nhỏ nhất có sẵn và quá trình quét sẽ giải quyết các mối quan hệ một cách tự nhiên bằng cách chọn chỉ mục nhỏ nhất trong số các giá trị bằng nhau nếu được thực hiện cẩn thận. Tuy nhiên, về mặt khái niệm, nó trở nên kém hiệu quả vì nó phải kiểm tra lại các phần tử đã được xử lý nhiều lần một cách dư thừa. 

Điều quan trọng cần lưu ý là quy tắc sắp xếp chính xác là những gì mà thuật toán sắp xếp tiêu chuẩn đã cung cấp nếu chúng ta đóng gói mỗi phần tử bằng chỉ mục của nó. Nếu chúng ta sắp xếp các cặp`(ai, i)`, so sánh sắp xếp trước tiên sử dụng độ khó và tự động quay trở lại thứ tự chỉ mục khi độ khó bằng nhau. Điều này chuyển vấn đề thành một cách sắp xếp duy nhất trên n mục. 

Sự cải tiến đến từ việc nhận ra rằng cấu trúc là tĩnh. Không có gì thay đổi trong quá trình xử lý, vì vậy chúng ta không cần lựa chọn tăng dần. Chỉ cần đặt hàng toàn cầu một lần là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Sắp xếp theo cặp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n và mảng độ khó. Mỗi vị trí ngầm thể hiện một mã định danh vấn đề bắt đầu từ 1. 
2. Xây dựng danh sách các cặp trong đó mỗi phần tử lưu trữ cả độ khó và chỉ số ban đầu của nó. Điều này bảo tồn danh tính của từng vấn đề trong khi cho phép so sánh. 
3. Sắp xếp danh sách các cặp. Quy tắc sắp xếp trước tiên sẽ so sánh độ khó và nếu hai độ khó bằng nhau thì quy tắc sẽ so sánh các chỉ số. Điều này đảm bảo thứ tự yêu cầu được thực thi trong một thao tác duy nhất. 
4. Duyệt qua danh sách đã sắp xếp và chỉ trích xuất các chỉ mục, tạo ra thứ tự đầu ra cuối cùng. 

### Tại sao nó hoạt động 

Việc sắp xếp áp đặt một trật tự toàn cục nhất quán với mối quan hệ thứ tự bắt buộc của bài toán. Hàm so sánh xác định tổng thứ tự trong đó`(ai, i)`đảm bảo rằng độ khó là khóa chính và chỉ mục là yếu tố quyết định. Sau khi được sắp xếp, hai phần tử bất kỳ sẽ xuất hiện theo đúng thứ tự tương đối và tính bắc cầu của việc sắp xếp đảm bảo toàn bộ chuỗi nhất quán mà không cần điều chỉnh thêm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

pairs = []
for i in range(n):
    pairs.append((a[i], i + 1))

pairs.sort()

res = [str(idx) for _, idx in pairs]
print(" ".join(res))
```Ý tưởng cốt lõi trong việc thực hiện là xây dựng`(value, index)`cặp. Chỉ mục được chuyển sang dựa trên 1 vì đầu ra mong đợi việc đánh số vấn đề bắt đầu từ 1. 

Tính năng sắp xếp tích hợp trong Python so sánh các bộ dữ liệu theo từ điển, nghĩa là nó tự động so sánh phần tử đầu tiên và chỉ khi chúng bằng nhau thì nó mới so sánh phần tử thứ hai. Điều này trực tiếp mã hóa quy tắc ràng buộc bắt buộc mà không cần thêm logic. 

Vòng lặp cuối cùng chỉ trích xuất các chỉ mục theo thứ tự, chuyển đổi chúng thành chuỗi để nối nhanh. sử dụng`sys.stdin.readline`đảm bảo việc xử lý đầu vào hiệu quả, mặc dù với n lên tới 1000 thì điều này không thực sự cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 1 4 2
```Chúng tôi tạo thành cặp: 

| Bước | Danh sách cặp | 
| --- | --- | 
| Ban đầu | (3,1), (1,2), (1,3), (4,4), (2,5) | 
| Sau khi sắp xếp | (1,2), (1,3), (2,5), (3,1), (4,4) | 

Các chỉ số được trích xuất:```
2 3 5 1 4
```Điều này cho thấy rằng các giá trị bằng nhau 1 và 1 giữ nguyên thứ tự chỉ số tương đối của chúng, thể hiện việc xử lý ràng buộc chính xác. 

### Ví dụ 2 

đầu vào:```
4
2 2 2 2
```Cặp: 

| Bước | Danh sách cặp | 
| --- | --- | 
| Ban đầu | (2,1), (2,2), (2,3), (2,4) | 
| Sau khi sắp xếp | (2,1), (2,2), (2,3), (2,4) | 

Đầu ra:```
1 2 3 4
```Điều này xác nhận rằng khi tất cả các giá trị giống hệt nhau, thuật toán sẽ giữ nguyên thứ tự ban đầu chính xác như yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp n cặp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ danh sách cặp | 

Với n lên tới 1000, việc sắp xếp tối đa 1000 phần tử là chuyện nhỏ. Thuật toán chạy thoải mái trong cả giới hạn thời gian và bộ nhớ, với giới hạn an toàn lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))
    pairs = [(a[i], i + 1) for i in range(n)]
    pairs.sort()
    return " ".join(str(idx) for _, idx in pairs)

# provided sample style case
assert run("5\n3 1 1 4 2\n") == "2 3 5 1 4"

# all equal
assert run("3\n5 5 5\n") == "1 2 3"

# already sorted
assert run("4\n1 2 3 4\n") == "1 2 3 4"

# reverse order
assert run("4\n4 3 2 1\n") == "4 3 2 1"

# minimum size
assert run("1\n7\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3 1 1 4 2 | 2 3 5 1 4 | Phân loại chung với xử lý ràng buộc | 
| 3 5 5 5 | 1 2 3 | Tất cả đều ổn định như nhau | 
| 4 1 2 3 4 | 1 2 3 4 | Trường hợp đã được sắp xếp | 
| 4 4 3 2 1 | 4 3 2 1 | Đặt hàng ngược lại | 
| 1 7 | 1 | Kích thước đầu vào tối thiểu | 

## Vỏ cạnh 

Khi mọi khó khăn đều giống nhau, chẳng hạn như đầu vào`5 5 5 5`, thuật toán xây dựng các phần tử đầu tiên giống hệt nhau trong tất cả các cặp. Việc sắp xếp sau đó hoàn toàn dựa trên các chỉ số, duy trì trật tự tự nhiên. Đầu ra trở thành`1 2 3 4 5`, phù hợp với yêu cầu mà các mối quan hệ được giải quyết theo vị trí ban đầu. 

Khi mảng tăng nghiêm ngặt, chẳng hạn như`1 2 3 4`, việc sắp xếp không làm thay đổi thứ tự của các cặp. Thuật toán vẫn thực hiện các bước tương tự nhưng kết quả đầu ra giống hệt các chỉ số đầu vào theo thứ tự. 

Khi mảng giảm nghiêm ngặt, chẳng hạn như`4 3 2 1`, việc sắp xếp đảo ngược hoàn toàn cấu trúc. Mỗi phần tử di chuyển đến vị trí chính xác dựa trên giá trị và vì các giá trị là khác nhau nên các chỉ số không ảnh hưởng đến thứ tự.
