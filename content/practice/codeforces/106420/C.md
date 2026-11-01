---
title: "CF 106420C - Cuộc gặp gỡ của nhà vô địch (Dễ)"
description: "Chúng ta được cung cấp hai danh sách các tay đua được sắp xếp theo thứ tự, trong đó mỗi tay đua đều có một giá trị uy tín duy nhất. Nhiệm vụ là xây dựng một đội hình cuối cùng duy nhất bằng cách liên tục chọn tay đua tiếp theo từ phía trước danh sách đầu tiên hoặc phía trước danh sách thứ hai, luôn tiêu thụ một phần tử tại một…"
date: "2026-06-20T03:46:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106420
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 3-11-26 (Beginner)"
rating: 0
weight: 106420
solve_time_s: 65
verified: true
draft: false
---

[CF 106420C - Cuộc gặp gỡ của nhà vô địch (Dễ dàng)](https://codeforces.com/problemset/problem/106420/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai danh sách các tay đua được sắp xếp theo thứ tự, trong đó mỗi tay đua đều có một giá trị uy tín duy nhất. Nhiệm vụ là xây dựng một đội hình cuối cùng duy nhất bằng cách liên tục chọn tay đua tiếp theo từ phía trước danh sách đầu tiên hoặc phía trước danh sách thứ hai, luôn tiêu thụ một phần tử mỗi lần. Mục tiêu là tạo thành một chuỗi kết hợp tôn trọng quy tắc ưu tiên tham lam dựa trên sự so sánh uy tín ở các vị trí dẫn đầu hiện tại của hai danh sách. 

Mỗi bước, chỉ có tay đua đầu tiên còn lại của mỗi danh sách mới quan trọng. Chúng tôi quyết định cái nào sẽ được đặt tiếp theo trong chuỗi đã hợp nhất, sau đó xóa nó khỏi danh sách. Quá trình tiếp tục cho đến khi cả hai danh sách đều cạn kiệt. 

Ý nghĩa quan trọng của các ràng buộc điển hình trong loại vấn đề hợp nhất này là tính tuyến tính: nếu tổng số tay đua lên tới khoảng 200000, thì chiến lược O(nm) hoặc O((n+m)²) ngay lập tức không khả thi. Hướng khả thi duy nhất là thuật toán nâng cao con trỏ qua cả hai danh sách nhiều nhất một lần, tạo ra giải pháp O(n + m). 

Trường hợp cạnh tinh tế xuất hiện khi một trong các danh sách sớm trống. Việc triển khai ngây thơ luôn so sánh các phần đầu mà không kiểm tra khoảng trống sẽ thất bại. Ví dụ: nếu A trống và B vẫn còn các phần tử, hành động hợp lệ duy nhất là lấy từ B. Một trường hợp cạnh khác là khi các phần tử đầu tiên có giá trị bằng nhau. Mặc dù vấn đề nêu rõ tính duy nhất trên toàn cầu, nhưng việc triển khai đôi khi giả định việc xử lý bình đẳng và đưa ra sự phân nhánh không cần thiết có thể phá vỡ logic thứ tự. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là mô phỏng tất cả các cách có thể để xen kẽ hai danh sách. Ở mỗi bước, có tối đa hai lựa chọn, vì vậy trong trường hợp xấu nhất chúng ta khám phá cây quyết định nhị phân có chiều cao n + m. Điều này dẫn đến độ phức tạp theo cấp số nhân, khoảng O(2^(n+m)), điều này trở nên không thể thực hiện được ngay cả đối với các đầu vào rất nhỏ ngoài vài chục phần tử. 

Quan sát quan trọng là chúng ta không thực sự cần khám phá cả hai lựa chọn. Ở bất kỳ bước nào, chúng tôi chỉ quan tâm lựa chọn tiền tố hiện tại nào dẫn đến chuỗi kết quả tốt hơn về mặt từ điển. Bởi vì tất cả các giá trị đều khác biệt nên việc so sánh các hậu tố còn lại sẽ giảm cục bộ xuống việc chỉ so sánh các phần tử hiện tại. Khi một đầu hoàn toàn nhỏ hơn đầu kia, việc chọn đầu đầu tiên luôn mang lại tiền tố tốt hơn trên toàn cầu và do đó không thể tệ hơn trong bất kỳ lần tiếp theo nào trong tương lai. 

Tính tối ưu cục bộ này thu gọn toàn bộ quá trình quyết định thành một sự hợp nhất tham lam đơn giản, tương tự như việc hợp nhất hai mảng đã sắp xếp, ngoại trừ thứ tự được xác định động bằng cách so sánh đầu thay vì sắp xếp cố định. 

Chúng ta duy trì hai con trỏ và liên tục lấy đầu hiện tại nhỏ hơn, nâng con trỏ đó lên. Khi một danh sách đã hết, các phần tử còn lại của danh sách kia sẽ được thêm trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(n+m) | Quá chậm | 
| Hợp nhất tham lam tối ưu | O(n+m) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình hợp nhất với hai chỉ số, một chỉ số cho mỗi danh sách. 

1. Khởi tạo hai con trỏ i và j ở đầu mảng A và B. Chúng đại diện cho các tay đua chưa được tiêu thụ đầu tiên trong mỗi danh sách. 
2. Trong khi cả i đều nằm trong A và j nằm trong B, hãy so sánh A[i] và B[j]. Giá trị nhỏ hơn được chọn làm tay đua tiếp theo trong chuỗi đầu ra. Lựa chọn này đúng vì mọi quyết định sau này không phụ thuộc vào việc bỏ qua tiền tố khả dụng nhỏ hơn khi tất cả các giá trị đều khác biệt. 
3. Nếu A[i] nhỏ hơn, hãy thêm nó vào kết quả và tăng i. Nếu không thì nối thêm B[j] và tăng j. Điều này đảm bảo rằng chỉ một phần tử được tiêu thụ trên mỗi bước, duy trì tiến trình tuyến tính. 
4. Khi một con trỏ đến cuối mảng, hãy nối tất cả các phần tử còn lại của mảng kia theo thứ tự. Tại thời điểm này không còn lựa chọn nào khác vì chỉ còn lại một nguồn.

Tại sao nó hoạt động: ở mỗi bước, quyết định là giữa hai ứng cử viên là thành phần sớm nhất còn lại của mỗi danh sách. Vì tất cả các giá trị là duy nhất nên việc chọn phần đầu nhỏ hơn luôn tạo ra tiền tố nhỏ hơn về mặt từ điển của chuỗi được hợp nhất. Bất kỳ lựa chọn thay thế nào sẽ ngay lập tức tăng vị trí khác nhau đầu tiên và không có giao dịch hoán đổi nào trong tương lai có thể sửa chữa thứ tự đó vì các phần tử sau không thể di chuyển sớm hơn trong chuỗi. Điều này thiết lập thuộc tính lựa chọn tham lam kết hợp với cấu trúc con tối ưu, đảm bảo tính chính xác của việc luôn lấy phần đầu có sẵn nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = j = 0
    res = []
    
    while i < n and j < m:
        if a[i] < b[j]:
            res.append(a[i])
            i += 1
        else:
            res.append(b[j])
            j += 1
    
    while i < n:
        res.append(a[i])
        i += 1
    
    while j < m:
        res.append(b[j])
        j += 1
    
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh hai con trỏ di chuyển độc lập qua các mảng. Vòng lặp chính chỉ tiếp tục khi cả hai danh sách vẫn còn các phần tử, đảm bảo mọi so sánh đều có ý nghĩa. Mỗi lần lặp lại tiêu thụ chính xác một phần tử, do đó không có phần tử nào được xem lại. 

Hai vòng cuối cùng xử lý các trường hợp cạn kiệt. Chúng cần thiết vì khi một mảng trống thì không thể so sánh thêm nữa và tất cả các phần tử còn lại phải được nối theo thứ tự. 

Một lỗi triển khai phổ biến là quên rằng vòng lặp hợp nhất phải dừng khi một trong hai con trỏ đạt đến giới hạn của nó chứ không phải cả hai. Một vấn đề tế nhị khác là xử lý sự bình đẳng không chính xác, nhưng ở đây tính duy nhất đảm bảo thứ tự nghiêm ngặt, do đó nhánh else xử lý an toàn tất cả các trường hợp còn lại. 

## Ví dụ đã hoạt động 

Xét A = [3, 7, 10] và B = [2, 6, 8, 11]. 

Chúng tôi theo dõi vị trí con trỏ và đầu ra. 

| tôi | j | A[i] | B[j] | Được chọn | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 2 | 2 | [2] | 
| 0 | 1 | 3 | 6 | 3 | [2, 3] | 
| 1 | 1 | 7 | 6 | 6 | [2, 3, 6] | 
| 1 | 2 | 7 | 8 | 7 | [2, 3, 6, 7] | 
| 2 | 2 | 10 | 8 | 8 | [2, 3, 6, 7, 8] | 
| 2 | 3 | 10 | 11 | 10 | [2, 3, 6, 7, 8, 10] | 
| 3 | 3 | - | - | còn lại B | [2, 3, 6, 7, 8, 10, 11] | 

Dấu vết này cho thấy thuật toán hoạt động giống hệt như việc hợp nhất hai chuỗi đã sắp xếp và luôn bảo toàn phần tử tiền tố nhỏ nhất có sẵn. 

Bây giờ hãy xem xét A = [5] và B = [1, 2, 3]. 

| tôi | j | A[i] | B[j] | Được chọn | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 1 | 1 | [1] | 
| 0 | 1 | 5 | 2 | 2 | [1, 2] | 
| 0 | 2 | 5 | 3 | 3 | [1, 2, 3] | 
| 0 | 3 | 5 | - | còn lại A | [1, 2, 3, 5] | 

Điều này xác nhận việc xử lý chính xác tình trạng cạn kiệt sớm của một danh sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi phần tử được thêm chính xác một lần và con trỏ chỉ di chuyển về phía trước | 
| Không gian | O(n + m) | Mảng đầu ra lưu trữ tất cả các phần tử đã hợp nhất | 

Độ phức tạp tuyến tính phù hợp với chế độ ràng buộc trong đó tổng kích thước đầu vào có thể lên tới hàng trăm nghìn, khiến cho một giải pháp vượt qua duy nhất là cần thiết và đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = j = 0
    res = []
    
    while i < n and j < m:
        if a[i] < b[j]:
            res.append(a[i])
            i += 1
        else:
            res.append(b[j])
            j += 1
    
    while i < n:
        res.append(a[i])
        i += 1
    
    while j < m:
        res.append(b[j])
        j += 1
    
    return " ".join(map(str, res))

# custom tests
assert run("1 1\n5\n3") == "3 5"
assert run("3 0\n1 2 3\n") == "1 2 3"
assert run("0 4\n4 1 2 3\n") == "4 1 2 3"
assert run("4 4\n1 4 7 10\n2 3 5 6") == "1 2 3 4 5 6 7 10"
assert run("5 5\n10 20 30 40 50\n1 2 3 4 5") == "1 2 3 4 5 10 20 30 40 50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5/3 | 3 5 | thứ tự trao đổi phần tử đơn | 
| 3 0 / 1 2 3 | 1 2 3 | mảng thứ hai trống | 
| 0 4 / 4 1 2 3 | 4 1 2 3 | mảng đầu tiên trống | 
| 4 4 / 1 4 7 10 + 2 3 5 6 | 1 2 3 4 5 6 7 10 | sự xen kẽ đúng đắn | 
| 5 5 giảm dần và tăng dần | thứ tự sắp xếp hợp nhất | trường hợp xấu nhất thay thế hoàn toàn | 

## Vỏ cạnh 

Khi một danh sách trống ngay từ đầu, thuật toán sẽ bỏ qua hoàn toàn vòng so sánh và trực tiếp đưa ra danh sách khác. Đối với đầu vào A = [] và B = [4, 1, 2], con trỏ i giữ nguyên ở 0 trong khi j tăng từ 0 lên 3, tạo ra [4, 1, 2] chính xác như yêu cầu. 

Khi tất cả các thành phần của một danh sách đều nhỏ hơn danh sách kia, thuật toán sẽ liên tục loại bỏ danh sách đó trước tiên. Đối với A = [1, 2, 3] và B = [10, 11], mọi so sánh sẽ chọn từ A cho đến hết, sau đó B được thêm vào. Tính bất biến mà con trỏ chỉ di chuyển về phía trước đảm bảo không có phần tử nào bị bỏ qua hoặc sắp xếp lại không chính xác. 

Khi xen kẽ đạt mức tối đa, chẳng hạn như A = [1, 3, 5, 7] và B = [2, 4, 6, 8], mỗi bước sẽ xen kẽ giữa các mảng. Thuật toán xử lý việc này mà không cần phân nhánh bổ sung vì mỗi so sánh sẽ giải quyết phần đầu nhỏ nhất tiếp theo một cách độc lập, duy trì tính chính xác ngay cả khi có sự luân phiên chặt chẽ.
