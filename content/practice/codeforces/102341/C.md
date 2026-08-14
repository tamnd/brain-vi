---
title: "CF 102341C - Cloyster"
description: "Chúng ta có một lưới (n lần n) và mỗi ô chứa một số nguyên riêng biệt biểu thị kích thước vỏ của Cloyster ở đó. Ô có giá trị lớn nhất là ô dẫn đầu. Chúng tôi không thể kiểm tra toàn bộ lưới vì các giá trị chỉ được tiết lộ khi chúng tôi truy vấn một ô một cách rõ ràng."
date: "2026-08-14T05:12:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "C"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 99
verified: true
draft: false
---

[CF 102341C - Cloyster](https://codeforces.com/problemset/problem/102341/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times n) và mỗi ô chứa một số nguyên riêng biệt biểu thị kích thước vỏ của Cloyster ở đó. Ô có giá trị lớn nhất là ô dẫn đầu. Chúng tôi không thể kiểm tra toàn bộ lưới vì các giá trị chỉ được tiết lộ khi chúng tôi truy vấn một ô một cách rõ ràng. 

Điều kiện bổ sung là thuộc tính cấu trúc quan trọng. Mỗi ô ngoại trừ mức tối đa toàn cầu đều có ít nhất một trong tám ô lân cận có giá trị lớn hơn. Vì tất cả các giá trị đều khác nhau nên việc di chuyển liên tục từ một ô sang bất kỳ ô lân cận lớn hơn nào cuối cùng phải đạt đến mức tối đa toàn cục duy nhất. 

Nhiệm vụ là xuất ra tọa độ tối đa đó trong khi sử dụng tối đa (3n+210) truy vấn. Giới hạn (n\le 2000) đủ nhỏ để quét một hàng hoặc cột hoàn chỉnh, nhưng không đủ lớn từ xa để quét toàn bộ lưới (n^2). Một giải pháp sử dụng truy vấn (O(n^2)) sẽ cần tới (4.000.000) truy vấn ở kích thước tối đa, vượt xa giới hạn tương tác. Do đó, mục tiêu hữu ích là tuyến tính trong (n), chỉ với số logarit của các kiểm tra lân cận có kích thước không đổi bổ sung. 

Thực tế là sự tương tác không thích ứng cũng có nghĩa là một ô luôn có cùng một câu trả lời khi được truy vấn nhiều lần. Chúng ta có thể khai thác điều đó bằng cách lưu vào bộ nhớ đệm mọi giá trị mà chúng ta đã thu được. 

Có một số trường hợp ranh giới có thể phá vỡ việc triển khai bất cẩn. Với (n=2), đường cắt ở giữa cũng là một ranh giới, do đó các vòng lặp lân cận không được truy vấn hàng (0), hàng (n+1), cột (0) hoặc cột (n+1). Ví dụ, với```
2
```và giá trị vỏ```
1 2
3 4
```câu trả lời đúng là ((2,2)). Việc triển khai giả định tồn tại cả hai mặt của mỗi dòng truy vấn có thể truy cập vào một ô không hợp lệ. 

Một trường hợp tinh vi khác xảy ra khi giá trị lớn nhất của dòng hiện đang được quét không phải là giá trị lớn nhất được phát hiện cho đến nay. Giả sử bước đệ quy trước đó đã phát hiện ra một giá trị (100), trong khi dòng phân cách tiếp theo chứa nhiều nhất các giá trị (90). Việc loại bỏ phần tốt nhất trước đó sẽ làm mất thông tin cho chúng ta biết nửa nào vẫn chứa mức tối đa toàn cục. Thuật toán phải giữ ô được truy vấn tốt nhất trên toàn cầu trong suốt quá trình đệ quy. 

Trường hợp hoàn toàn bình đẳng được yêu cầu kiểm tra không phải là trường hợp có vấn đề pháp lý. Các ràng buộc ban đầu yêu cầu tất cả các kích thước shell phải khác nhau và thuộc tính lân cận lớn hơn cũng phụ thuộc vào thứ tự nghiêm ngặt đó. Đối với một đầu vào nhân tạo như```
2
5 5
5 5
```không có người lãnh đạo duy nhất, vì vậy không có đầu ra chính xác nào tồn tại theo quy tắc của bài toán. Khai thác thử nghiệm sẽ coi trường hợp đó là không hợp lệ thay vì mong đợi một tọa độ cụ thể. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là truy vấn từng ô và ghi nhớ giá trị lớn nhất. Điều đó đúng vì người dẫn đầu chính xác là mức tối đa toàn cầu duy nhất. Số truy vấn trong trường hợp xấu nhất của nó là (n^2), đạt tới (4.000.000) khi (n=2000). Sự tương tác chỉ cho phép (3n+210), tối đa là (6210), vì vậy việc tìm kiếm toàn diện là không thể. 

Quan sát hữu ích là điều kiện láng giềng lớn hơn cho chúng ta một con đường ngày càng tăng từ mọi người không phải người lãnh đạo đến người lãnh đạo. Hãy tưởng tượng vẽ một dấu phân cách qua hình chữ nhật hiện tại, một hàng hoàn chỉnh hoặc một cột hoàn chỉnh. Truy vấn mọi ô trên dấu phân cách đó và lấy giá trị tối đa của nó, chẳng hạn như (X). 

Nếu dấu phân cách là một hàng thì mọi hàng xóm ngang của (X) sẽ nhỏ hơn vì (X) là hàng tối đa. Do đó, bất kỳ hàng xóm lớn hơn nào của (X) đều phải ở hàng trên hoặc hàng dưới. Nếu không có lân cận lớn hơn thì (X) đã là cực đại cục bộ và điều kiện của bài toán hàm ý rằng nó phải là cực đại toàn cục. 

Nếu có một ô lân cận lớn hơn (Y), hướng từ (X) đến (Y) cho chúng ta biết phía nào chứa mức tối đa toàn cục. Lý do là thuộc tính đường dẫn tăng dần. Bắt đầu từ (Y), liên tục di chuyển tới hàng xóm lớn hơn. Các giá trị tăng nghiêm ngặt nên đường dẫn này không thể đi qua dấu phân cách qua (X), có giá trị nhỏ hơn (Y) và nó không thể đi qua một ô phân tách khác vì mọi giá trị dấu phân cách khác đều lớn nhất là (X). Do đó đường đi tới cực đại toàn cục vẫn nằm ở phía chứa (Y). 

Đối số tương tự hoạt động với dấu phân cách cột. Một cột tối đa chỉ có thể có hàng xóm lớn hơn ở bên trái hoặc bên phải của nó, vì vậy chúng tôi loại bỏ nửa đối diện. 

Có một chi tiết bổ sung khi đệ quy đã giới hạn một chiều. Ô được truy vấn tốt nhất từ ​​dấu phân cách trước đó phải vẫn có sẵn. Chúng tôi duy trì giá trị tối đa được thấy trong toàn bộ tương tác và tọa độ của nó. Sau khi truy vấn một dấu phân cách mới và vùng lân cận của nó, kết quả tốt nhất toàn cục đó sẽ cho chúng ta biết nên lặp lại bên nào. Điều này tránh việc dựa vào thuộc tính lân cận lớn hơn ban đầu bên trong hình chữ nhật được cắt xén nhân tạo. 

Để giữ số lượng truy vấn tuyến tính, hãy luôn cắt kích thước dài hơn. Khi đó, dấu phân cách có giá trị tối đa bằng chiều dài của kích thước ngắn hơn và kích thước ngắn hơn đó bằng khoảng một nửa kích thước dài hơn trước đó sau khi cắt hàng và cột xen kẽ. Chuỗi hình học thu được được giới hạn bởi (3n), trong khi tối đa sáu lân cận chưa biết trước đó xung quanh mỗi dấu phân cách chỉ đóng góp tối đa (O(\log n)) truy vấn bổ sung. Giới hạn được thiết lập là (3n+12\log_2 n), ở dưới (3n+210) cho (n\le2000). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) truy vấn | (O(n^2)) nếu được lưu vào bộ nhớ đệm | Quá chậm | 
| Tối ưu | (O(n+\log n)) truy vấn | (O(n^2)) với bộ đệm đơn giản | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc (n) và bắt đầu với toàn bộ hình chữ nhật ([1,n]\times[1,n]). Duy trì tọa độ truy vấn ánh xạ bộ đệm theo kích thước shell của chúng. Đồng thời duy trì giá trị lớn nhất được truy vấn cho đến nay và tọa độ của nó. 
2. Nếu hình chữ nhật hiện tại chỉ chứa một ô thì ô đó chính là đáp án. Xuất tọa độ của nó và chấm dứt. 
3. So sánh chiều cao và chiều rộng của hình chữ nhật. Nếu chiều rộng ít nhất bằng chiều cao thì chọn cột giữa làm dấu phân cách. Nếu không thì chọn hàng giữa. Việc cắt kích thước dài hơn đảm bảo rằng kích thước bị giảm ít nhất phải lớn bằng dòng chúng ta truy vấn, điều này làm cho truy vấn tuyến tính bị ràng buộc. 
4. Truy vấn từng ô trên dấu phân cách đã chọn và tìm giá trị cũng như vị trí tối đa của nó. Đưa giá trị đó vào mức tốt nhất trên toàn cầu cho đến nay. Các ô được truy vấn trước đó được trả về từ bộ nhớ đệm mà không cần sử dụng một truy vấn tương tác nào khác. 
5. Truy vấn tối đa sáu ô lân cận của dấu phân cách tối đa nằm ở phía bên kia của dấu phân cách. Đối với dấu phân cách hàng, đây là các ô ở các hàng ngay bên trên và bên dưới và ba cột xung quanh mức tối đa. Đối với dấu phân cách cột, tình huống là đối xứng. Cập nhật tốt nhất toàn cầu sau mỗi truy vấn như vậy. 
6. Nếu giá trị tốt nhất toàn cục nằm trên dấu phân cách, hãy xuất tọa độ của nó. Mức tối đa phân cách không có lân cận lớn hơn là mức tối đa cục bộ và mọi không phải cấp cao nhất đều có lân cận lớn hơn, vì vậy mức tối đa cục bộ này phải là mức tối đa cục bộ. 
7. Nếu không, hãy kiểm tra xem phía nào của dải phân cách chứa giá trị tốt nhất toàn cầu. Nếu nó nằm phía trên dấu phân cách hàng, hãy lặp lại ở nửa trên. Nếu nó ở dưới, lặp lại ở nửa dưới. Đối với dấu phân cách cột, hãy lặp lại ở nửa bên trái hoặc bên phải. 
8. Lặp lại cho đến khi còn lại một ô hoặc chính dấu phân cách chứa ô tốt nhất toàn cục. 

### Tại sao nó hoạt động 

Điều bất biến là mức tối đa toàn cục luôn nằm bên trong hình chữ nhật hiện tại và ô được truy vấn lớn nhất toàn cầu cũng nằm bên trong hình chữ nhật đó. Hãy xem xét một dấu phân cách hàng và đặt (X) là giá trị tối đa của nó. Nếu giá trị được truy vấn tốt nhất hiện tại nằm phía trên dấu phân cách thì sẽ có một ô được truy vấn (Y>X) phía trên nó. Từ (Y), liên tục theo sau một ô lân cận lớn hơn cuối cùng phải đạt mức tối đa toàn cục. Đường đi tăng dần như vậy không thể đi qua dấu phân cách, bởi vì mọi ô trên dấu phân cách không phải (X) đều nhỏ hơn (X<Y) và bản thân (X) cũng nhỏ hơn (Y). Do đó mức tối đa toàn cầu phải ở trên dấu phân cách. Trường hợp cột giống hệt nhau. 

Nếu không có ô lân cận nào lớn hơn (X), (X) là mức tối đa cục bộ. Mọi ô không tối đa được đảm bảo có ô lân cận lớn hơn, vì vậy (X) không thể là ô không tối đa. Do đó, nó là mức tối đa toàn cầu duy nhất. 

## Giải pháp Python 

Sau đây là cách thực hiện tương tác thực tế. Nó dành cho trình tương tác Codeforces, không dành cho đầu vào tệp thông thường. Vấn đề chính thức có tính tương tác rõ ràng và mỗi truy vấn phải được xóa ngay lập tức.```python
import sys
input = sys.stdin.readline

cache = {}
best_value = -1
best_x = -1
best_y = -1
query_count = 0

def query(x, y):
    global query_count

    if (x, y) in cache:
        return cache[(x, y)]

    print("?", x, y, flush=True)
    value = int(input())

    # A negative response is normally used by an interactor to signal
    # an invalid query or failure.
    if value < 0:
        sys.exit(0)

    cache[(x, y)] = value
    query_count += 1
    return value

def update_best(x, y):
    global best_value, best_x, best_y

    value = query(x, y)
    if value > best_value:
        best_value = value
        best_x = x
        best_y = y

def solve(u, d, l, r, n):
    if u == d and l == r:
        print("!", u, l, flush=True)
        sys.exit(0)

    # Cut the longer dimension.
    if d - u < r - l:
        # Vertical separator.
        m = (l + r) // 2

        x = u
        value = query(x, m)

        for i in range(u + 1, d + 1):
            cur = query(i, m)
            if cur > value:
                value = cur
                x = i

        update_best(x, m)

        # Check the neighbors on the two sides of the separator.
        for i in range(max(x - 1, 1), min(x + 1, n) + 1):
            if m > 1:
                update_best(i, m - 1)
            if m < n:
                update_best(i, m + 1)

        if best_y == m:
            print("!", x, m, flush=True)
            sys.exit(0)

        if best_y < m:
            solve(u, d, l, m - 1, n)
        else:
            solve(u, d, m + 1, r, n)

    else:
        # Horizontal separator.
        m = (u + d) // 2

        y = l
        value = query(m, y)

        for j in range(l + 1, r + 1):
            cur = query(m, j)
            if cur > value:
                value = cur
                y = j

        update_best(m, y)

        # Check the neighbors on the two sides of the separator.
        for j in range(max(y - 1, 1), min(y + 1, n) + 1):
            if m > 1:
                update_best(m - 1, j)
            if m < n:
                update_best(m + 1, j)

        if best_x == m:
            print("!", m, y, flush=True)
            sys.exit(0)

        if best_x < m:
            solve(u, m - 1, l, r, n)
        else:
            solve(m + 1, d, l, r, n)

def main():
    n = int(input())
    solve(1, n, 1, n, n)

if __name__ == "__main__":
    main()
```các`cache`từ điển triển khai thuộc tính tương tác không thích ứng. Nếu một ô lân cận đã được truy vấn như một phần của dấu phân cách trước đó, việc yêu cầu lại ô đó sẽ không tạo ra một ô khác`?`lời yêu cầu. Điều này đặc biệt hữu ích vì các dấu phân cách liên tiếp có chung các vùng lân cận ranh giới.`update_best`được cố tình tách khỏi`query`. Hàm đầu tiên nhận một giá trị và sau đó cập nhật mức tối đa toàn cầu, trong khi hàm thứ hai chỉ chịu trách nhiệm liên lạc và lưu vào bộ nhớ đệm. Việc giữ những trách nhiệm này tách biệt làm cho sự bất biến xung quanh`best_x`,`best_y`, Và`best_value`dễ bảo quản hơn. 

Khi chiều rộng lớn hơn chiều cao, mã sẽ chọn cột ở giữa và quét các hàng của nó. Khi chiều cao ít nhất bằng, nó chọn hàng ở giữa và quét các cột của nó. Việc ràng buộc đi đến trường hợp hàng, điều này là tùy ý và không ảnh hưởng đến tính chính xác. 

Các vòng lặp lân cận sử dụng`max`Và`min`so với ranh giới lưới thực tế. Điều này quan trọng đối với cực đại của dải phân cách nằm trên một cạnh hoặc một góc. Không có lo ngại về tràn số nguyên trong Python và các giá trị shell đủ nhỏ để số nguyên thông thường là quá đủ. 

Hình chữ nhật đệ quy sử dụng tọa độ bao gồm. Do đó, nửa bên trái của việc phân chia cột là`[l, m-1]`và nửa bên phải là`[m+1, r]`. Bản thân dấu phân cách bị loại trừ vì thông tin liên quan của nó đã được truy vấn. Quy tắc tương tự áp dụng cho việc chia hàng. 

các`flush=True`đối số là bắt buộc đối với một giải pháp tương tác. Nếu không có nó, chương trình có thể đợi phản hồi của người tương tác trong khi truy vấn của nó vẫn được lưu vào bộ đệm. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên của câu lệnh là bản ghi tương tác chứ không phải là bài kiểm tra đầu vào/đầu ra thông thường. Người tương tác đưa ra (n=3), sau đó trả lời năm truy vấn của chương trình với các giá trị (1,4,8,9,5). Câu trả lời cuối cùng là ((3,3)). Bản ghi âm được hiển thị trên trang vấn đề ban đầu. 

Đối với dấu vết khái niệm, các giá trị được truy vấn mẫu trả về là: 

| Truy vấn | Tế bào | Giá trị trả về | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| 1 | ((1,1)) | 1 | 1 tại ((1,1)) | 
| 2 | ((2,3)) | 4 | 4 tại ((2,3)) | 
| 3 | ((3,2)) | 8 | 8 tại ((3,2)) | 
| 4 | ((3,3)) | 9 | 9 tại ((3,3)) | 
| 5 | ((2,2)) | 5 | 9 tại ((3,3)) | 
| Trả lời | ((3,3)) | 9 | 9 tại ((3,3)) | 

Phần quan trọng của mẫu này là việc truy vấn một tập hợp ô tùy ý có thể tiết lộ ô dẫn đầu, ngay cả khi không tuân theo thứ tự tìm kiếm tối ưu chính xác. Sau khi tìm thấy giá trị được truy vấn tối đa và cấu trúc xung quanh xác nhận giá trị đó, chương trình có thể kết thúc. 

Mẫu thứ hai có (n=5) và trình tương tác trả về (2) cho một truy vấn tại ((4,4)). Câu lệnh chính thức cho phép chương trình đoán ngay cả khi thông tin được truy vấn không xác định được câu trả lời một cách logic, do đó bản ghi 

| Hành động | Tế bào | Giá trị trả về | Kết quả | 
| --- | --- | --- | --- | 
| Truy vấn | ((4,4)) | 2 | Chỉ có một giá trị được biết | 
| Trả lời | ((1,1)) | | Chương trình chấm dứt | 

được chấp nhận bởi bộ tương tác của mẫu đó. 

Mẫu thứ hai này thể hiện thuộc tính của giao thức tương tác thay vì bằng chứng tìm kiếm xác định. Nó cũng giải thích tại sao việc sao chép mẫu dưới dạng thử nghiệm đơn vị ngoại tuyến thông thường là không phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+\log n)) truy vấn | Quét phân tách tạo thành một chuỗi hình học, trong khi mỗi cấp độ đệ quy chỉ thêm một số lượng truy vấn lân cận không đổi | 
| Không gian | (O(n^2)) trường hợp xấu nhất | Bộ đệm có thể chứa mọi tọa độ được truy vấn, mặc dù số lượng truy vấn thực tế chỉ là (O(n)) | 

Đối với (n\le2000), giới hạn truy vấn tối đa là khoảng (3n+12\log_2n), nằm dưới mức an toàn (3n+210). Do đó, việc triển khai vẫn nằm trong giới hạn tương tác. Công việc thực tế của CPU cũng rất nhỏ so với giới hạn 2 giây. Các tài nguyên chính thức của vấn đề đưa ra cùng một kiểu (3n+12\log_2 n) cho chiến lược này.

 ## Trường hợp thử nghiệm 

Vì đây là sự cố tương tác nên các mẫu được cung cấp không thể được chuyển trực tiếp đến chương trình đã gửi như stdin thông thường. Một thử nghiệm tự động thích hợp cần một trình tương tác mô phỏng cung cấp các giá trị bất cứ khi nào giải pháp in truy vấn. Khai thác sau đây kiểm tra logic tìm kiếm ngoại tuyến bằng cách thay thế`query`với truy cập ma trận trực tiếp trong khi vẫn giữ nguyên thuật toán đệ quy.```python
# Offline simulation of the interactive algorithm.
# The real Codeforces submission must use the interactive query() function.

def run_matrix(a):
    n = len(a)

    cache = {}
    best_value = -1
    best_x = -1
    best_y = -1

    def query(x, y):
        if (x, y) not in cache:
            cache[(x, y)] = a[x - 1][y - 1]
        return cache[(x, y)]

    def update_best(x, y):
        nonlocal best_value, best_x, best_y

        value = query(x, y)
        if value > best_value:
            best_value = value
            best_x = x
            best_y = y

    def solve(u, d, l, r):
        nonlocal best_value, best_x, best_y

        if u == d and l == r:
            return u, l

        if d - u < r - l:
            m = (l + r) // 2

            x = u
            value = query(x, m)

            for i in range(u + 1, d + 1):
                cur = query(i, m)
                if cur > value:
                    value = cur
                    x = i

            update_best(x, m)

            for i in range(max(x - 1, 1), min(x + 1, n) + 1):
                if m > 1:
                    update_best(i, m - 1)
                if m < n:
                    update_best(i, m + 1)

            if best_y == m:
                return x, m

            if best_y < m:
                return solve(u, d, l, m - 1)
            return solve(u, d, m + 1, r)

        else:
            m = (u + d) // 2

            y = l
            value = query(m, y)

            for j in range(l + 1, r + 1):
                cur = query(m, j)
                if cur > value:
                    value = cur
                    y = j

            update_best(m, y)

            for j in range(max(y - 1, 1), min(y + 1, n) + 1):
                if m > 1:
                    update_best(m - 1, j)
                if m < n:
                    update_best(m + 1, j)

            if best_x == m:
                return m, y

            if best_x < m:
                return solve(u, m - 1, l, r)
            return solve(m + 1, d, l, r)

    return solve(1, n, 1, n)

# Custom 1: minimum-size valid grid.
a1 = [
    [1, 2],
    [3, 4],
]
assert run_matrix(a1) == (2, 2), "minimum-size grid"

# Custom 2: maximum at the top-left boundary.
a2 = [
    [16, 15, 14, 13],
    [12, 11, 10, 9],
    [8, 7, 6, 5],
    [4, 3, 2, 1],
]
assert run_matrix(a2) == (1, 1), "top-left boundary"

# Custom 3: maximum at the bottom-right boundary.
a3 = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
    [13, 14, 15, 16],
]
assert run_matrix(a3) == (4, 4), "bottom-right boundary"

# Custom 4: maximum away from the boundaries.
a4 = [
    [1, 2, 3, 4, 5],
    [6, 7, 8, 9, 10],
    [11, 12, 25, 14, 15],
    [16, 17, 18, 19, 20],
    [21, 22, 23, 24, 13],
]
assert run_matrix(a4) == (3, 3), "interior maximum"

# Deliberately invalid according to the original problem.
# All values are equal, so there is no unique leader.
invalid_equal = [
    [5, 5],
    [5, 5],
]
# No assertion is made for invalid_equal because the original problem
# guarantees distinct values.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[[1,2],[3,4]]`|`(2,2)`| Tối thiểu (n), góc tối đa, kiểm tra ranh giới | 
|`[[16,15,14,13],...,[4,3,2,1]]`|`(1,1)`| Ranh giới trên cùng bên trái và đệ quy hướng lên trên | 
|`[[1,2,3,4],...,[13,14,15,16]]`|`(4,4)`| Ranh giới dưới cùng bên phải và đệ quy đi xuống | 
|`5 x 5`lưới với`25`Tại`(3,3)`|`(3,3)`| Điểm cuối cực đại và dải phân cách bên trong | 
|`[[5,5],[5,5]]`| Không hợp lệ | Xác nhận rằng dữ liệu hoàn toàn bằng nhau vi phạm các đảm bảo về vấn đề | 

## Vỏ cạnh 

Đối với trường hợp (2\times2)```
1 2
3 4
```dấu phân cách đầu tiên là một cột vì các kích thước được gắn và việc triển khai sẽ chọn nhánh hàng trong tình huống đó. Nó quét hàng (1), tìm (2), kiểm tra các ô ngay bên dưới nó, phát hiện (4) và ghi lại ((2,2)) là ô được biết đến nhiều nhất. Sau đó đệ quy di chuyển xuống nửa dưới và đến góc chính xác. Bộ phận bảo vệ ranh giới ngăn chặn mọi truy vấn bên ngoài lưới. 

Để có mức tối đa ở góc trên bên trái, hãy xem xét```
16 15 14 13
12 11 10 9
8  7  6  5
4  3  2  1
```Dấu phân cách tối đa gặp phải trong quá trình tìm kiếm cuối cùng có một hàng xóm lớn hơn được biết đến ở phía dẫn về góc trên bên trái. Các tọa độ tốt nhất toàn cầu được giữ lại trong khi hình chữ nhật co lại, do đó, thuật toán không bao giờ mất ứng cử viên ngay cả khi các dòng phân cách sau này chứa các giá trị nhỏ hơn. 

Để đạt mức tối đa ở góc dưới bên phải,```
1  2  3  4
5  6  7  8
9  10 11 12
13 14 15 16
```cơ chế tương tự hoạt động theo hướng ngược lại. Bất cứ khi nào dấu phân cách hiện tại hiển thị giá trị lớn hơn ở phía dưới hoặc bên phải, nửa tương ứng sẽ được giữ lại. Câu trả lời cuối cùng chỉ nằm ở hình chữ nhật còn lại. 

Để đạt được mức tối đa bên trong,```
1  2  3  4  5
6  7  8  9  10
11 12 25 14 15
16 17 18 19 20
21 22 23 24 13
```bản thân mức phân cách tối đa có thể là phần dẫn đầu. Sau khi thuật toán truy vấn các ô lân cận và không có ô nào lớn hơn, đối số tối đa cục bộ sẽ được áp dụng ngay lập tức. Không cần đệ quy thêm nữa. 

Cuối cùng, một lưới hoàn toàn bằng nhau như```
5 5
5 5
```không được sử dụng như một bài kiểm tra vấn đề hợp lệ. Câu lệnh đảm bảo kích thước vỏ riêng biệt theo cặp, do đó không có ô lớn nhất duy nhất ở đây. Giải pháp trả về một trong bốn tọa độ sẽ chỉ đưa ra lựa chọn tùy ý chứ không giải quyết được vấn đề đã chỉ định.
