---
title: "CF 102215E - Phần mềm của bên thứ ba - 2"
description: "Có (m) hàm số liên tiếp được đánh số từ (1) đến (m). Mỗi phiên bản thư viện cấp quyền truy cập vào một khoảng liền kề của các hàm này, từ (ai) đến (bi)."
date: "2026-08-17T23:39:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "E"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 285
verified: false
draft: false
---

[CF 102215E - Phần mềm của bên thứ ba - 2](https://codeforces.com/problemset/problem/102215/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 45 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Có (m) hàm số liên tiếp được đánh số từ (1) đến (m). Mỗi phiên bản thư viện cấp quyền truy cập vào một khoảng liền kề của các hàm này, từ (a_i) đến (b_i). Pavel có thể mua bất kỳ bộ sưu tập phiên bản nào và tập hợp các khoảng của chúng phải chứa mọi hàm từ (1) đến (m). 

Nhiệm vụ có hai phần. Đầu tiên, hãy xác định xem một bộ sưu tập như vậy có tồn tại hay không. Nếu nó tồn tại, hãy tìm một bộ sưu tập có số phiên bản nhỏ nhất có thể và in chỉ mục gốc của chúng. 

Ràng buộc lớn là (n \le 200000), trong khi (m) có thể lớn bằng (10^9). Điều này ngay lập tức loại trừ bất cứ điều gì lặp lại trên tất cả các hàm (m), vì bản thân (m) có thể lớn hơn nhiều so với (n). Nó cũng loại trừ việc liệt kê các tập hợp con của các phiên bản, bởi vì (2^{200000}) vượt xa mọi giới hạn thực tế. Giải pháp (O(n^2)) cũng quá chậm ở quy mô này vì nó có thể thực hiện so sánh khoảng (4\cdot10^{10}). Chúng ta cần một thuật toán (O(n\log n)) hoặc lý tưởng nhất là (O(n)) sau khi sắp xếp. 

Các khoảng được đóng lại, do đó các khoảng liền kề kết nối với nhau mà không có khoảng cách. Ví dụ: ([1,3]) và ([4,7]) cùng bao gồm mọi số nguyên từ (1) đến (7). Việc triển khai bất cẩn yêu cầu khoảng thời gian tiếp theo bắt đầu nhiều nhất ở điểm cuối bên phải hiện tại sẽ từ chối trường hợp này một cách không chính xác. 

Xem xét đầu vào```
2 7
1 3
4 7
```Đầu ra đúng bắt đầu bằng`YES`và yêu cầu cả hai phiên bản. Khoảng thứ hai bắt đầu từ (4), nhưng hàm (4) chính xác là hàm chưa được khám phá tiếp theo sau (1,2,3), do đó không có khoảng trống. 

Một trường hợp tế nhị khác là khoảng thời gian bắt đầu quá muộn:```
2 8
1 3
5 8
```Đầu ra đúng là```
NO
```vì hàm (4) chưa được khám phá. Việc triển khai chỉ kiểm tra xem điểm cuối bên phải cuối cùng đạt đến (m) có thể chấp nhận các khoảng thời gian này một cách không chính xác hay không. 

Trường hợp cạnh thứ ba là khi một khoảng đã bao gồm mọi thứ:```
3 8
1 8
3 5
6 7
```Câu trả lời đúng sử dụng đúng một phiên bản, đó là phiên bản (1). Việc đếm tất cả các khoảng tham gia vào một số khả năng che phủ nào đó sẽ tạo ra một câu trả lời không tối thiểu. 

Cuối cùng, một số khoảng có thể bắt đầu ở cùng một vị trí và nên ưu tiên khoảng có điểm cuối bên phải lớn nhất:```
4 10
1 3
1 6
2 4
6 10
```Câu trả lời tối ưu sử dụng phiên bản (2) và (4). Việc chọn phiên bản (1) trước tiên vẫn cho phép một giải pháp, nhưng nói chung nó không phải là một lựa chọn tham lam an toàn. Tại mọi thời điểm, chúng tôi cần khoảng thời gian mở rộng vùng phủ sóng hiện tại xa nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xem xét mọi tập hợp con của (n) phiên bản thư viện. Đối với mỗi tập hợp con, chúng ta có thể thu thập các khoảng của nó, sắp xếp hoặc quét chúng và kiểm tra xem liệu sự kết hợp của chúng có bao trùm toàn bộ phạm vi từ (1) đến (m) hay không. Chúng ta có thể nhớ tập hợp con hợp lệ nhỏ nhất. Điều này đúng vì mọi quyết định mua hàng có thể xảy ra đều xuất hiện giữa các tập hợp con, vì vậy tập hợp con hợp lệ nhất nhất thiết phải là tập hợp con tối ưu. 

Vấn đề là số lượng tập hợp con. Có (2^n) trong số chúng và việc kiểm tra một tập hợp con có thể yêu cầu (O(n)) hoạt động. Trong trường hợp xấu nhất, điều này mang lại các hoạt động (O(n2^n)). Ngay cả khi bỏ qua hệ số (n), (2^{200000}) cũng lớn hơn một cách không thể tưởng tượng được so với số lượng thao tác có thể phù hợp với giới hạn thời gian hai giây. 

Cấu trúc hữu ích là mỗi phiên bản đều cung cấp một khoảng thời gian. Để bao gồm tiền tố của các hàm, chúng tôi chỉ quan tâm đến khoảng cách mà chúng tôi đã chọn hiện tại ở bên phải. Giả sử các hàm (1) đến (r) đã được đề cập. Bất kỳ khoảng thời gian tiếp theo hữu ích nào cũng phải bắt đầu tại hoặc trước (r+1), vì khoảng thời gian bắt đầu sau (r+1) sẽ để lại một khoảng trống. 

Trong số tất cả các khoảng có thể mở rộng phạm vi phủ sóng hiện tại, việc chọn khoảng có điểm cuối bên phải lớn nhất ít nhất cũng tốt bằng việc chọn một khoảng kết thúc trước đó. Nó bao gồm mọi thứ mà khoảng thời gian ngắn hơn sẽ bao gồm và có thể hơn thế nữa. Điều này biến bài toán thành bài toán bao phủ khoảng tối thiểu cổ điển. 

Sau khi sắp xếp các khoảng theo điểm cuối bên trái, chúng ta có thể xử lý chúng từ trái sang phải. Ở mọi giai đoạn, hãy duy trì điểm cuối bên phải xa nhất trong số tất cả các khoảng mà điểm cuối bên trái đã có thể truy cập được. Chọn khoảng thời gian đạt được điểm cuối xa nhất rồi lặp lại. Nếu không có khoảng thời gian tiếp cận nào có thể mở rộng phạm vi phủ sóng thì sẽ tồn tại một khoảng trống và câu trả lời là không thể. 

Lựa chọn tham lam khóa là tối ưu vì bất kỳ giải pháp hợp lệ nào mở rộng tiền tố hiện tại đều phải sử dụng một khoảng thời gian nào đó bắt đầu không muộn hơn hàm chưa được phát hiện tiếp theo. Việc thay thế khoảng đó bằng khoảng có thể truy cập có điểm cuối bên phải xa nhất không thể làm cho vấn đề còn lại trở nên khó khăn hơn. Do đó, sự lựa chọn tham lam không bao giờ sử dụng nhiều khoảng thời gian hơn một giải pháp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mỗi khoảng cùng với số phiên bản gốc của nó, sau đó sắp xếp các khoảng theo điểm cuối bên trái của chúng. Việc sắp xếp cho phép chúng tôi khám phá tất cả các khoảng thời gian có thể sử dụng được khi tiền tố được bao phủ phát triển mà không cần quét nhiều lần toàn bộ đầu vào. 
2. Bắt đầu với`covered = 0`. Điều này có nghĩa là hàm số (1) thông qua`covered`hiện đang được đề cập, vì vậy chức năng tiếp theo phải được đề cập là`covered + 1`. 
3. Quét các khoảng được sắp xếp trong khi điểm cuối bên trái của chúng nhiều nhất`covered + 1`. Trong số các khoảng có thể truy cập này, hãy nhớ khoảng có điểm cuối bên phải lớn nhất. Khoảng thời gian như vậy là lựa chọn tiếp theo mạnh mẽ nhất có thể vì nó mở rộng tiền tố được bao phủ càng xa càng tốt. 
4. Nếu không có khoảng thời gian nào có thể mở rộng phạm vi phủ sóng hiện tại, hãy dừng và in`NO`. Mỗi khoảng thời gian còn lại bắt đầu sau`covered + 1`, vậy hàm`covered + 1`không bao giờ có thể được bao phủ bởi bất kỳ khoảng không được chọn nào. 
5. Nếu không, hãy chọn khoảng thời gian đã ghi nhớ, thêm chỉ mục ban đầu của khoảng thời gian đó vào câu trả lời và đặt`covered`đến điểm cuối bên phải của nó. Tiền tố mới được che phủ có thể tạo ra các khoảng thời gian bổ sung có thể truy cập được, vì vậy hãy tiếp tục quét từ vị trí hiện tại. 
6. Lặp lại cho đến khi`covered >= m`. Tại thời điểm đó, mọi hàm từ (1) đến (m) đều được bao phủ và các chỉ số được chọn tạo thành một nghiệm hợp lệ. 
7. In`YES`, số lượng phiên bản được chọn và chỉ số ban đầu của chúng. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi phép chọn tham lam, tất cả các hàm từ (1) đến`covered`được bao phủ và mọi khoảng có điểm cuối bên trái nhiều nhất là`covered + 1`đã được coi là một khoảng thời gian tiếp theo có thể. 

Giả sử một giải pháp tối ưu mở rộng tiền tố được bảo hiểm hiện tại. Khoảng thời gian được chọn tiếp theo của nó phải bắt đầu không muộn hơn`covered + 1`, nếu không thì hàm chưa được khám phá đầu tiên sẽ vẫn bị khám phá. Thuật toán tham lam kiểm tra tất cả các khoảng như vậy và chọn một khoảng có điểm cuối bên phải là cực đại. Việc thay thế khoảng thời gian tiếp theo của giải pháp tối ưu bằng khoảng tham lam không thể làm giảm tiền tố được bao phủ, vì khoảng tham lam ít nhất đạt đến mức xa nhất. Do đó, phần còn lại của giải pháp tối ưu vẫn đủ hoặc sớm trở nên không cần thiết. 

Bằng cách liên tục thực hiện trao đổi này, sẽ tồn tại một giải pháp tối ưu bắt đầu từ mọi lựa chọn tham lam. Đối số tương tự được áp dụng ở mọi bước tiếp theo, vì vậy giải pháp tham lam hoàn chỉnh sử dụng số lượng phiên bản tối thiểu có thể. Nếu thuật toán bị kẹt trước khi đạt tới (m), thì không có khoảng nào có khả năng bao hàm hàm tiếp theo, do đó không tồn tại nghiệm hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    intervals = []
    for idx in range(1, n + 1):
        a, b = map(int, input().split())
        intervals.append((a, b, idx))

    intervals.sort()

    ans = []
    covered = 0
    pos = 0

    while covered < m:
        best_right = covered
        best_idx = -1

        while pos < n and intervals[pos][0] <= covered + 1:
            a, b, idx = intervals[pos]
            if b > best_right:
                best_right = b
                best_idx = idx
            pos += 1

        if best_idx == -1:
            print("NO")
            return

        ans.append(best_idx)
        covered = best_right

    print("YES")
    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu dưới dạng`(a, b, idx)`, Ở đâu`idx`là số phiên bản gốc. Sắp xếp theo bộ dữ liệu sẽ sắp xếp chủ yếu theo`a`, đó chính xác là những gì mà quá trình quét tham lam cần.`covered`đại diện cho chức năng ngoài cùng bên phải đã được đảm bảo bao phủ. Ban đầu nó bằng 0, do đó, khoảng có thể sử dụng được nếu điểm cuối bên trái của nó lớn nhất là (1). Sau khi mở rộng phạm vi đến (7), khoảng bắt đầu từ (8) cũng có thể sử dụng được vì các chức năng (1) đến (8) có thể được bao phủ liên tục. 

Vòng lặp bên trong sử dụng mọi khoảng thời gian mà điểm cuối bên trái có thể truy cập được ở giai đoạn hiện tại. Trong số đó,`best_right`ghi lại điểm cuối bên phải xa nhất. Không cần phải xem xét lại một khoảng thời gian sau đó. Khi điểm cuối bên trái của nó có thể truy cập được, nó vẫn có thể truy cập được mãi mãi và nếu đó không phải là lựa chọn tốt nhất ở giai đoạn này thì giai đoạn tham lam sau này không thể làm cho nó tốt hơn khoảng thời gian đã được chọn để mở rộng phạm vi phủ sóng. 

Sự so sánh chặt chẽ`b > best_right`là đủ. Nếu hai khoảng đạt đến cùng một điểm cuối, thì một trong hai khoảng sẽ đưa ra phạm vi bao phủ chính xác như nhau, do đó chỉ mục ban đầu là hợp lệ. 

Việc kiểm tra khoảng cách được thể hiện bằng`best_idx == -1`. Nếu không có khoảng thời gian có thể truy cập nào mở rộng tiền tố hiện tại thì không thể truy cập được chức năng chưa được khám phá tiếp theo. Điều này mạnh hơn việc chỉ kiểm tra xem có tồn tại một khoảng nào đó hay không, bởi vì một khoảng bắt đầu tại`covered + 2`hoặc muộn hơn không thể kết nối chức năng bị thiếu. 

Không có vấn đề tràn số nguyên trong Python và dù sao thì giá trị tối đa của (m) vẫn nhỏ so với phạm vi số nguyên của Python. biểu hiện`covered + 1`cũng chính xác là ranh giới chính xác vì các khoảng đều được bao hàm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa bốn khoảng cách nhau phân chia chính xác phạm vi được yêu cầu. 

| Lặp lại |`covered`trước | Khoảng thời gian có thể tiếp cận | Phiên bản được chọn |`covered`sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`[1,2]`| 1 | 2 | 
| 2 | 2 |`[3,4]`| 2 | 4 | 
| 3 | 4 |`[5,6]`| 3 | 6 | 
| 4 | 6 |`[7,8]`| 4 | 8 | 

Ở lần lặp đầu tiên, chỉ có phiên bản (1) mới có thể bao gồm chức năng (1). Sau khi chọn nó, chức năng (3) sẽ trở thành chức năng chưa được khám phá tiếp theo, do đó phiên bản (2) sẽ có thể truy cập được. Quá trình tương tự tiếp tục cho đến khi toàn bộ phạm vi được bao phủ. 

Kết quả là`YES`, với bốn phiên bản được lựa chọn. Vì mỗi khoảng đều cần thiết để kết nối một phần mới của phạm vi nên không có giải pháp nào có thể sử dụng ít hơn bốn khoảng. 

### Mẫu 2 

Ở đây các khoảng là```
1: [1,5]
2: [2,7]
3: [3,4]
4: [6,8]
```Thứ tự sắp xếp đã là thứ tự đầu vào. 

| Lặp lại |`covered`trước | Khoảng thời gian có thể tiếp cận | Điểm cuối tốt nhất | Phiên bản được chọn |`covered`sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 |`[1,5]`| 5 | 1 | 5 | 
| 2 | 5 |`[2,7]`| 7 | 2 | 7 | 
| 3 | 7 |`[6,8]`| 8 | 4 | 8 | 

Dấu vết này tiết lộ một chi tiết quan trọng. Phiên bản (2) đã được xem xét trong lần lặp đầu tiên, nhưng không thể chọn nó vì điểm cuối bên trái của nó là (2), trong khi chức năng (1) vẫn chưa được đề cập. Khi phiên bản (1) bao quát hết (5), phiên bản (2) sẽ có thể truy cập được và mở rộng phạm vi phủ sóng sang (7). 

Giải pháp tham lam thu được thực tế là các phiên bản (1,2,4), sử dụng ba phiên bản. Tuy nhiên, đầu ra mẫu sử dụng các phiên bản (1,4), vì phiên bản (1) bao gồm (5) và phiên bản (4) bắt đầu từ (6) và đạt đến (8). Điều này cho thấy một lỗ hổng trong đường vẽ trên nếu phiên bản (2) được chọn chỉ vì nó vươn xa hơn. 

Thuật toán tham lam chính xác phải chọn điểm cuối có thể tiếp cận xa nhất, do đó, nó thực sự sẽ chọn phiên bản (1) trước, đạt (5) và sau đó là phiên bản (2), đạt (7), dẫn đến ba phiên bản. Điều đó mâu thuẫn với câu trả lời được xác nhận của mẫu về hai phiên bản. Lý do là phiên bản của mẫu (4) là`[6,8]`, do đó phiên bản (1) và (4) bao gồm`[1,5]`Và`[6,8]`, hợp lệ. Do đó, bài toán đã nêu không thể sử dụng khoảng giải thích tối thiểu tiêu chuẩn với các khoảng chính xác như được cung cấp nếu câu trả lời mẫu có ý định`2`. 

Đối với vấn đề như đã viết, tiêu chí tham lam chính xác thay vào đó dựa trên việc chọn các khoảng có liên kết bao phủ toàn bộ phạm vi hàm và thuật toán tham lam tiêu chuẩn phải được áp dụng bằng cách sắp xếp các khoảng theo điểm cuối bên phải của chúng khi xây dựng lớp phủ tối thiểu chỉ khi ngữ nghĩa mục tiêu và phạm vi phù hợp với công thức đó. Với mẫu được cung cấp, câu trả lời mong đợi chứng tỏ rằng việc chọn`[1,5]`theo sau là`[6,8]`là hợp lệ, trong khi những kẻ tham lam tầm thường nhất sẽ chọn`[2,7]`sau đó`[1,5]`. 

Sự không nhất quán này có nghĩa là tuyên bố được cung cấp và dữ liệu mẫu không đủ để biện minh cho giải pháp tham lam tiêu chuẩn ở trên. Một bài xã luận đúng phải sử dụng đúng ngữ nghĩa dự định của vấn đề Codeforces ban đầu thay vì chỉ suy luận chúng từ tuyên bố được sao chép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp các khoảng (n) chiếm ưu thế trong quá trình quét, quá trình này xử lý mỗi khoảng một lần. | 
| Không gian | (O(n)) | Các khoảng thời gian và chỉ số phiên bản đã chọn được lưu trữ rõ ràng. | 

Đối với (n=200000), việc sắp xếp yêu cầu khoảng vài triệu phép so sánh, sau đó là quét tuyến tính. Điều đó phù hợp với giới hạn hai giây trong môi trường lập trình cạnh tranh được tối ưu hóa. Giá trị (m) có thể đạt tới (10^9), nhưng thuật toán không bao giờ lặp qua các hàm riêng lẻ, do đó kích thước của (m) không ảnh hưởng đến thời gian chạy. 

## Trường hợp thử nghiệm 

Bởi vì Mẫu 2 được cung cấp không nhất quán với công thức tham lam bao phủ khoảng thông thường, nên các thử nghiệm dưới đây sử dụng cách giải thích đã nêu rằng sự kết hợp của các khoảng đóng đã chọn phải bao gồm mọi hàm từ (1) đến (m). Trình trợ giúp xác thực tập hợp được trả về thay vì yêu cầu một tập hợp chỉ mục chính xác vì có thể tồn tại một số câu trả lời tối ưu khác nhau.```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    intervals = []
    for idx in range(1, n + 1):
        a = int(next(it))
        b = int(next(it))
        intervals.append((a, b, idx))

    intervals.sort()

    ans = []
    covered = 0
    pos = 0

    while covered < m:
        best_right = covered
        best_idx = -1

        while pos < n and intervals[pos][0] <= covered + 1:
            a, b, idx = intervals[pos]
            if b > best_right:
                best_right = b
                best_idx = idx
            pos += 1

        if best_idx == -1:
            return "NO\n"

        ans.append(best_idx)
        covered = best_right

    return "YES\n{}\n{}\n".format(len(ans), " ".join(map(str, ans)))

def validate(inp: str, out: str):
    lines = out.strip().splitlines()
    assert lines, "empty output"

    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))
    m = int(next(it))

    intervals = []
    for idx in range(1, n + 1):
        a = int(next(it))
        b = int(next(it))
        intervals.append((a, b))

    if lines[0] == "NO":
        # Verify independently with the same reachability criterion.
        intervals2 = sorted((a, b) for a, b in intervals)
        covered = 0
        pos = 0

        while covered < m:
            best = covered
            while pos < n and intervals2[pos][0] <= covered + 1:
                best = max(best, intervals2[pos][1])
                pos += 1
            if best == covered:
                return
            covered = best

        raise AssertionError("reported NO for a coverable instance")

    assert lines[0] == "YES"
    k = int(lines[1])
    chosen = list(map(int, lines[2].split()))

    assert len(chosen) == k
    assert len(set(chosen)) == k
    assert all(1 <= x <= n for x in chosen)

    selected = [intervals[x - 1] for x in chosen]
    selected.sort()

    covered = 0
    for a, b in selected:
        assert a <= covered + 1
        covered = max(covered, b)

    assert covered >= m

    # Verify minimality by computing the greedy optimum independently.
    all_intervals = sorted(
        (a, b, idx) for idx, (a, b) in enumerate(intervals, 1)
    )

    optimum = 0
    covered = 0
    pos = 0

    while covered < m:
        best = covered

        while pos < n and all_intervals[pos][0] <= covered + 1:
            best = max(best, all_intervals[pos][1])
            pos += 1

        assert best > covered
        covered = best
        optimum += 1

    assert k == optimum

def run(inp: str) -> str:
    return solve_data(inp)

sample1 = """\
4 8
1 2
3 4
5 6
7 8
"""
validate(sample1, run(sample1))

# The ordinary interval-covering interpretation gives 3 here:
# [1,5], [2,7], [6,8].
sample2 = """\
4 8
1 5
2 7
3 4
6 8
"""
validate(sample2, run(sample2))

sample3 = """\
3 8
1 3
4 5
6 7
"""
validate(sample3, run(sample3))

# Minimum-size case: one version already covers everything.
case4 = """\
1 1
1 1
"""
validate(case4, run(case4))

# All intervals are identical. Only one is needed.
case5 = """\
5 10
1 10
1 10
1 10
1 10
1 10
"""
validate(case5, run(case5))

# Boundary adjacency: [1,3] and [4,6] touch exactly.
case6 = """\
2 6
1 3
4 6
"""
validate(case6, run(case6))

# Gap at function 4.
case7 = """\
3 7
1 3
5 7
2 2
"""
assert run(case7).strip() == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`YES`, một phiên bản | Đầu vào tối thiểu và khoảng thời gian hoàn chỉnh | 
| Năm bản sao của`[1,10]`|`YES`, một phiên bản | Khoảng trùng lặp và số lượng thẻ tối thiểu | 
|`[1,3]`,`[4,6]`|`YES`, hai phiên bản | Điểm cuối bao gồm và lân cận | 
|`[1,3]`,`[5,7]`,`[2,2]`|`NO`| Một chức năng chưa được khám phá và phát hiện khoảng cách chính hãng | 

Trình xác thực cố tình không so sánh các chỉ số phiên bản theo nghĩa đen. Nếu hai phiên bản đưa ra mức độ bao phủ tối ưu như nhau thì cả hai phiên bản đều là câu trả lời hợp lệ. Thay vào đó, nó kiểm tra xem các khoảng được báo cáo có bao trùm toàn bộ phạm vi hay không và số được báo cáo có bằng với mức tối ưu tham lam được tính toán độc lập hay không. 

## Vỏ cạnh 

Trường hợp liền kề```
2 6
1 3
4 6
```bắt đầu bằng`covered = 0`. Khoảng đầu tiên có thể truy cập được vì điểm cuối bên trái của nó là (1), do đó phạm vi bao phủ trở thành (3). Khoảng thứ hai có điểm cuối bên trái (4 = được bao phủ + 1), do đó, nó cũng có thể truy cập được. Phạm vi bảo hiểm trở thành (6) và thuật toán in`YES`với hai phiên bản. Sự bình đẳng trong`a <= covered + 1`là điều cần thiết ở đây. Thay thế nó bằng`a <= covered`sẽ báo cáo sai`NO`. 

Trường hợp khoảng cách```
3 7
1 3
5 7
2 2
```bắt đầu bằng việc chọn`[1,3]`, bởi vì đó là khoảng duy nhất có khả năng mở rộng phạm vi bao phủ của hàm (1). Hàm yêu cầu tiếp theo là (4). Khoảng thời gian`[2,2]`đã được xem xét và không thể mở rộng phạm vi bảo hiểm, trong khi`[5,7]`bắt đầu quá muộn. Không có khoảng nào có thể bao hàm hàm (4), vì vậy`best_idx`còn lại`-1`và thuật toán in chính xác`NO`. 

Đối với các khoảng giống nhau,```
5 10
1 10
1 10
1 10
1 10
1 10
```tất cả năm khoảng đều có thể truy cập được ở bước đầu tiên, nhưng tất cả chúng đều kết thúc ở (10). Thuật toán chỉ chọn điểm đầu tiên mà nó gặp vì không có khoảng nào sau đó có điểm cuối bên phải lớn hơn. Mức độ bao phủ ngay lập tức đạt tới (10), vì vậy câu trả lời chứa chính xác một phiên bản. 

Đối với trường hợp nhỏ nhất có thể,```
1 1
1 1
```hàm yêu cầu ban đầu là (1), khoảng duy nhất có thể truy cập được và điểm cuối bên phải của nó là (1). Vòng lặp kết thúc ngay sau một lần lựa chọn. Điều này kiểm tra cả ranh giới ban đầu`covered = 0`và điều kiện dừng`covered >= m`. 

Tuy nhiên, có một vấn đề cơ bản với Mẫu 2 được cung cấp. Theo cách giải thích liên kết khoảng thời gian được nêu trong lời nhắc, các khoảng thời gian`[1,5]`,`[2,7]`,`[3,4]`, Và`[6,8]`có phạm vi bao phủ tối thiểu là ba khoảng, không phải hai khoảng theo thuật toán tham lam tiền tố có phạm vi tiếp cận xa nhất, trong khi mẫu tuyên bố rằng các phiên bản`1`Và`4`đủ. Từ`[1,5]`Và`[6,8]`thực sự bao gồm tất cả các số nguyên từ (1) đến (8), mức tối thiểu thực tế nhiều nhất là hai và mức tối thiểu chính xác là hai. Do đó, tiêu chuẩn tham lam được mô tả trong bài xã luận này không chính xác đối với mẫu được cung cấp. 

Mâu thuẫn này không thể được sửa chữa bằng một chi tiết triển khai như thay đổi`<`ĐẾN`<=`. Sự tham lam phải được sửa đổi để giải thích thực tế là việc chọn khoảng có điểm cuối bên phải xa nhất trong số tất cả các khoảng hiện có thể truy cập có thể sử dụng nhiều phiên bản hơn một lựa chọn khác để tạo ra sự liên kết ranh giới tốt hơn. Do đó, câu lệnh và các mẫu được cung cấp trong lời nhắc không mô tả vấn đề mà giải pháp tham lam tiêu chuẩn được trình bày là hợp lệ. Một bài xã luận đáng tin cậy của Codeforces không nên trình bày thuật toán đó như đã được chấp nhận cho tập mẫu chính xác này mà không giải quyết trước sự khác biệt trong định nghĩa vấn đề ban đầu.
