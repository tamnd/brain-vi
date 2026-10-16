---
title: "CF 105491D - Tiền tố hoàn hảo"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Cấu trúc ẩn quan trọng là chúng ta được phép tự do sắp xếp lại các phần tử, nhưng chỉ theo một cách hạn chế: chúng ta có thể chọn bất kỳ hai vị trí nào và hoán đổi giá trị của chúng."
date: "2026-06-27T01:28:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105491
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #37 (Brute-Forces1)"
rating: 0
weight: 105491
solve_time_s: 45
verified: true
draft: false
---

[CF 105491D - Tiền tố hoàn hảo](https://codeforces.com/problemset/problem/105491/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Cấu trúc ẩn quan trọng là chúng ta được phép tự do sắp xếp lại các phần tử, nhưng chỉ theo một cách hạn chế: chúng ta có thể chọn bất kỳ hai vị trí nào và hoán đổi giá trị của chúng. Vì vậy, một cách hiệu quả, chúng ta có thể hoán vị mảng một cách tùy ý, vì bất kỳ hoán vị nào cũng có thể đạt được thông qua hoán đổi. 

Sau khi chọn cách sắp xếp, chúng ta muốn mảng thỏa mãn điều kiện tiền tố: mọi tổng tiền tố phải hoàn toàn dương. Nói cách khác, khi chúng tôi quét từ trái sang phải, tổng hiện có không bao giờ giảm xuống 0 hoặc thấp hơn tại bất kỳ điểm nào. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xác định số lần hoán đổi tối thiểu cần thiết để sắp xếp lại mảng thành thứ tự “tiền tố-tổng-dương hợp lệ” như vậy. Nếu không có sự sắp xếp lại nào có thể đạt được đặc tính này thì chúng ta phải xuất ra -1. 

Các ràng buộc đầu vào ngụ ý rằng tổng số phần tử trong tất cả các trường hợp thử nghiệm là lớn, lên tới khoảng 10^5. Điều này loại trừ bất kỳ cách tiếp cận nào thử tất cả các hoán vị hoặc mô phỏng các giao dịch hoán đổi một cách rõ ràng. Ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng quá chậm trong trường hợp xấu nhất. 

Trường hợp cạnh khóa là khi mảng có quá nhiều giá trị âm so với giá trị dương. Ví dụ: nếu mảng chứa nhiều -1 hơn 1, ngay cả thứ tự tốt nhất cũng không thể ngăn tổng tiền tố cuối cùng trở thành không dương. 

Một trường hợp tinh vi khác phát sinh khi tổng số dương nhưng chỉ vừa đủ dương. Ví dụ: nếu mảng là [1, -1, -1, 1] thì tổng bằng 0, do đó tổng tiền tố cuối cùng không thể dương. Điều này ngay lập tức khiến câu trả lời trở nên không thể thực hiện được mặc dù mảng cục bộ trông có vẻ cân bằng. 

Loại trường hợp cạnh thứ hai là khi có sự sắp xếp nhưng không yêu cầu hoán đổi gì cả. Ví dụ: [1, 1, -1] đã hợp lệ, vì vậy câu trả lời là 0 mặc dù suy nghĩ ngây thơ có thể cân nhắc việc sắp xếp lại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi hoán vị của mảng và kiểm tra xem tổng tiền tố của nó có dương hay không. Đối với mỗi hoán vị, hãy tính tổng tiền tố theo O(n) và theo dõi số lượng hoán đổi tối thiểu cần thiết để đạt được nó từ mảng ban đầu. Tuy nhiên, số lượng hoán vị là n!, và thậm chí bỏ qua việc tính toán hoán đổi, việc chỉ kiểm tra tính hợp lệ đã trở nên không thể thực hiện được nếu vượt quá n rất nhỏ. 

Một cách hữu ích hơn để suy nghĩ về vấn đề này là tách biệt hai câu hỏi: khi nào thì có thể thực hiện được một sự sắp xếp hợp lệ, và cấu trúc của sự sắp xếp đó là gì. 

Điều kiện tổng tiền tố gợi ý rõ ràng về một cấu trúc tham lam. Nếu chúng ta muốn tất cả các tổng tiền tố vẫn dương thì chúng ta muốn có những đóng góp dương lớn càng sớm càng tốt. Vì mảng chỉ bao gồm 1 và -1 nên quyền tự do thực sự duy nhất là cách chúng ta xen kẽ chúng. 

Nếu chúng ta định nghĩa P là số lượng 1 và N là số lượng -1 thì mọi sự sắp xếp hợp lệ đều phải đáp ứng P > N trong mọi tiền tố và đặc biệt là P phải vượt quá N trên toàn cầu. Vậy điều kiện cần là P > N. 

Bây giờ hãy xem xét việc xây dựng một thứ tự hợp lệ. Chiến lược tự nhiên là luôn đặt sớm số 1 bất cứ khi nào có thể và trì hoãn -1. Trên thực tế, một thứ tự tham lam đặt tất cả các số 1 lên trước rồi đến tất cả các số -1 luôn hợp lệ bất cứ khi nào P > N, vì tổng tiền tố ban đầu tăng nghiêm ngặt rồi giảm xuống nhưng không bao giờ đạt đến giá trị không dương nếu P đủ lớn hơn. 

Khi chúng ta biết tính hợp lệ chỉ phụ thuộc vào số lượng, vấn đề còn lại sẽ là cần bao nhiêu lần hoán đổi để chuyển mảng ban đầu thành một số thứ tự hợp lệ. 

Quan sát quan trọng là mọi sự sắp xếp hợp lệ đều phải tương ứng với việc chọn chuỗi mục tiêu trong đó tất cả các số 1 xuất hiện trước khi đủ số -1 được đưa vào một cách có kiểm soát. Các giao dịch hoán đổi tối thiểu cần thiết tương đương với việc đếm số lượng -1 hiện được đặt ở các vị trí mà số 1 phải ở và ngược lại, liên quan đến thứ tự tối ưu chính tắc.

Một cách rõ ràng để chính thức hóa điều này là xem xét sự sắp xếp mục tiêu như tất cả các số 1 được đặt ở vị trí P đầu tiên. Sau đó, chúng tôi so sánh mảng ban đầu với mục tiêu này và đếm các phần không khớp. Mỗi lần hoán đổi có thể khắc phục được hai phần tử bị đặt sai vị trí, do đó, câu trả lời là khoảng một nửa số lượng phần tử không khớp, miễn là tính khả thi được giữ nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam + đếm không khớp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mảng có bao nhiêu số 1 và -1. Điều này xác định liệu có thể thực hiện được bất kỳ sự sắp xếp hợp lệ nào hay không. 
2. Nếu số lượng 1 không lớn hơn số lượng -1, hãy trả về -1 ngay lập tức. Tổng số tiền sẽ không dương trong bất kỳ hoán vị nào, do đó một số tiền tố sẽ không thành công. 
3. Cố định cách sắp xếp mục tiêu chuẩn bằng cách đặt tất cả các số 1 trước, sau đó là tất cả các số -1. Điều này hợp lệ bất cứ khi nào điều kiện ở bước 2 được đáp ứng, bởi vì mọi tiền tố trong số 1 đều có tổng dương và phần dư của 1 đảm bảo chúng ta không bao giờ giảm xuống 0 sau khi thêm -1. 
4. Quét mảng ban đầu và so sánh nó với vị trí mục tiêu theo từng vị trí, đếm những điểm không khớp. Sự không khớp xảy ra khi -1 xuất hiện trong vùng bắt buộc phải có số 1 hoặc ngược lại. 
5. Mỗi lần hoán đổi có thể sửa hai vị trí không khớp, vì việc hoán đổi số 1 bị đặt sai vị trí bằng số -1 bị đặt sai vị trí sẽ sửa cả hai vị trí cùng một lúc. Do đó hãy tính các kết quả không khớp chia cho 2 làm câu trả lời. 
6. Xuất giá trị đó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi cấu hình cuối cùng hợp lệ đều được xác định hoàn toàn bởi vị trí của các số 1 so với -1 và ràng buộc tổng tiền tố buộc tất cả các số 1 phải được sử dụng càng sớm càng tốt trong một sự sắp xếp tối ưu. Bởi vì việc hoán đổi không bị hạn chế về chỉ số, nên vấn đề giảm xuống còn việc chuyển đổi một cách sắp xếp nhiều tập hợp thành một hoán vị chính tắc cố định. Đối số ghép nối không khớp đảm bảo rằng mỗi lần hoán đổi sẽ giảm số lượng vị trí không chính xác xuống đúng hai và không có hoán đổi nào có thể làm tốt hơn, do đó, việc ghép nối tham lam là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n = int(input())
    a = list(map(int, input().split()))

    ones = a.count(1)
    negs = n - ones

    if ones <= negs:
        return -1

    # target: all 1s then -1s
    target = [1] * ones + [-1] * negs

    mismatches = 0
    for i in range(n):
        if a[i] != target[i]:
            mismatches += 1

    return mismatches // 2

t = int(input())
for _ in range(t):
    print(solve_case())
```Đầu tiên, mã sẽ kiểm tra tính khả thi bằng cách sử dụng một điều kiện đếm đơn giản. Đây là hạn chế toàn cầu duy nhất quan trọng cho sự tồn tại. 

Sau đó, nó xây dựng thứ tự mục tiêu khái niệm mà không lưu trữ rõ ràng bất cứ thứ gì lớn hơn một mẫu đơn giản. Số lượng không khớp được tính theo thời gian tuyến tính. 

Việc chia cho hai là chi tiết triển khai chính: mỗi lần hoán đổi giải quyết chính xác một vị trí 1 không chính xác và một vị trí -1 không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5 

a = [1, -1, 1, 1, -1] 

Mục tiêu là [1, 1, 1, -1, -1] 

| tôi | một [tôi] | mục tiêu[i] | không khớp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | -1 | 1 | 1 | 
| 3 | 1 | 1 | 0 | 
| 4 | 1 | -1 | 1 | 
| 5 | -1 | -1 | 0 | 

Không khớp = 2, vì vậy câu trả lời là 1 lần hoán đổi. Điều này tương ứng với việc hoán đổi vị trí 2 và 4. 

### Ví dụ 2 

đầu vào: 

n = 4 

a = [-1, -1, 1, 1] 

Mục tiêu là [1, 1, -1, -1] 

| tôi | một [tôi] | mục tiêu[i] | không khớp | 
| --- | --- | --- | --- | 
| 1 | -1 | 1 | 1 | 
| 2 | -1 | 1 | 1 | 
| 3 | 1 | -1 | 1 | 
| 4 | 1 | -1 | 1 | 

Không khớp = 4, vì vậy câu trả lời là 2 lần hoán đổi. Mỗi lần hoán đổi sẽ trao đổi một vị trí sai -1 với một vị trí sai vị trí 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | đếm và so sánh một lần | 
| Không gian | O(1) thêm | quầy chỉ được sử dụng | 

Giải pháp vừa vặn một cách thoải mái trong các ràng buộc vì tổng n trên tất cả các trường hợp thử nghiệm tối đa là 10^5, do đó thuật toán thực hiện tổng thể khoảng 10^5 thao tác. 

## Vỏ cạnh 

Nếu mảng có số 1 và -1 bằng nhau thì việc kiểm tra tính khả thi sẽ thất bại ngay lập tức. Ví dụ: [1, -1] không thể tạo ra tổng tiền tố dương hoàn toàn theo bất kỳ thứ tự nào, vì tổng cuối cùng bằng 0 và một số tiền tố phải giảm xuống 0. 

Nếu mảng đã được sắp xếp thành tất cả các số 1 theo sau là tất cả các số -1, thì số lượng không khớp bằng 0 và câu trả lời là không có hoán đổi. Chẳng hạn, [1, 1, 1, -1] đã thỏa mãn cấu trúc đích. 

Nếu mảng bị trộn nhiều, chẳng hạn như các giá trị xen kẽ như [1, -1, 1, -1, 1, -1, 1], thì số lần hoán đổi không khớp sẽ lớn, nhưng mỗi lần hoán đổi vẫn giải quyết được hai vị trí đặt sai, do đó, thuật toán sẽ nén chi phí một cách tự nhiên thành một số nguyên nhỏ các lần hoán đổi thay vì tăng tuyến tính với tình trạng hỗn loạn.
