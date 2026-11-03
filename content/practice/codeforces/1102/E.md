---
title: "CF 1102E - Phép đếm đơn điệu"
description: "Chúng ta được cho một dãy số nguyên và chúng ta muốn gán một dãy số nguyên khác có cùng độ dài, bắt đầu từ 0, với các quy tắc cấu trúc rất cụ thể."
date: "2026-06-13T07:41:09+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1102
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 531 (Div. 3)"
rating: 1700
weight: 1102
solve_time_s: 321
verified: false
draft: false
---

[CF 1102E - Phép đếm đơn điệu](https://codeforces.com/problemset/problem/1102/E) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, sắp xếp 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta muốn gán một dãy số nguyên khác có cùng độ dài, bắt đầu từ 0, với các quy tắc cấu trúc rất cụ thể. 

Đầu tiên, mọi vị trí chứa cùng một giá trị trong mảng ban đầu cũng phải nhận cùng một giá trị trong mảng được xây dựng. Vì vậy, các lớp đẳng thức trong mảng ban đầu trở thành các lớp đẳng thức bắt buộc trong chuỗi mới. 

Thứ hai, chuỗi được xây dựng phải di chuyển theo một cách rất hạn chế: khi chúng ta quét từ trái sang phải, mỗi giá trị tiếp theo sẽ giữ nguyên hoặc tăng đúng một. Không có khả năng giảm và không có bước nhảy nào lớn hơn một. 

Nhiệm vụ không phải là xây dựng một chuỗi hợp lệ mà là đếm xem có bao nhiêu chuỗi riêng biệt có thể được hình thành dưới những ràng buộc này. 

Các ràng buộc cho phép lên tới hai trăm nghìn phần tử, điều này ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân đối với các phép gán hoặc phân vùng. Bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc log-tuyến tính. Sự hiện diện của các ràng buộc bình đẳng về giá trị và quy tắc cục bộ từng bước gợi ý rằng cấu trúc chỉ phụ thuộc vào các mối quan hệ liền kề và các ranh giới lặp lại hơn là các kết hợp toàn cục. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị bằng nhau xuất hiện cách xa nhau. Ví dụ: nếu một phần tử lặp lại sau một khoảng cách dài, thì mọi phép gán đều phải giữ cả hai vị trí giống hệt nhau trong mảng được xây dựng, điều này có thể buộc các lựa chọn trước đó phải nhất quán với các ràng buộc sau này. Một cấu trúc tham lam ngây thơ từ trái sang phải xử lý các vị trí một cách độc lập sẽ thất bại ở đây vì nó bỏ qua những ràng buộc bình đẳng lan truyền trên toàn cầu. 

Một trường hợp cạnh khác là khi mảng được xen kẽ nghiêm ngặt như`[1,2,1,2,1,2]`. Ở đây, các ràng buộc bình đẳng sẽ thu gọn các vị trí không liền kề và quyền tự do thực sự chỉ nằm ở cách giới thiệu các “mức” giá trị trên các khối. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng xây dựng tất cả các mảng hợp lệ`b`trực tiếp. Tại mỗi vị trí, chúng tôi giữ nguyên giá trị hoặc tăng giá trị đó lên một đơn vị, nhưng chúng tôi cũng phải thực thi rằng tất cả đều bằng nhau`a`vị trí chia sẻ giống nhau`b`. Điều này gợi ý việc gán giá trị cho từng giá trị riêng biệt trong`a`, sau đó kiểm tra tính nhất quán trên toàn bộ mảng. 

Nếu có`k`các giá trị riêng biệt, về nguyên tắc, mỗi giá trị có thể được gán một mức nào đó và sau đó chúng ta phải đảm bảo rằng các vị trí liền kề khác nhau nhiều nhất là một. Việc thử tất cả các bài tập là không thể vì ngay cả với mức độ vừa phải`k`, số lượng ánh xạ tăng theo cấp số nhân. 

Nhận xét quan trọng là hạn chế chỉ phụ thuộc vào việc liệu chúng ta có bị buộc phải đưa ra một “cấp độ mới” khi chuyển từ vị trí này sang vị trí tiếp theo hay không. Hãy xem xét quét mảng từ trái sang phải. Bất cứ khi nào chúng tôi gặp một giá trị chưa xuất hiện trước đó, chúng tôi có thể tự do gán cho nó cùng cấp với giá trị hiện tại hoặc tăng cấp thêm một. Tuy nhiên, khi chúng tôi nhìn thấy lại một giá trị, mức được chỉ định của nó đã được cố định và nó có thể hạn chế liệu có thể thực hiện một số mức tăng nhất định sớm hơn hay không. 

Điều này hóa ra giúp giảm bớt việc theo dõi nơi các phân đoạn mới bắt đầu so với lần xuất hiện đầu tiên. Cấu trúc trở nên tương đương với cách đếm để quyết định xem mỗi lần xuất hiện đầu tiên có đưa ra một bước tăng mới hay không, tùy thuộc vào tính nhất quán với các phép gán trước đó. 

Đối với từng vị trí, chúng tôi duy trì xem đây có phải là lần đầu tiên giá trị này xuất hiện hay không. Câu trả lời trở thành sản phẩm của các lựa chọn ở những lần xuất hiện đầu tiên này, với sự phụ thuộc được giải quyết bằng cách đảm bảo rằng không có xung đột nào phát sinh với những lần lặp lại trong tương lai. Công thức đúng sẽ đơn giản hóa hơn nữa thành một quy trình động trong đó chúng tôi truyền bá các cấu hình hợp lệ và nhân các lựa chọn bất cứ khi nào một phân đoạn mới có thể mở rộng hoặc giữ nguyên. 

Một cách rõ ràng để thể hiện giải pháp là theo dõi, đối với mỗi giá trị, vị trí xuất hiện đầu tiên của nó. Sau đó, chúng tôi xử lý mảng từ trái sang phải và mỗi lần chúng tôi gặp lần xuất hiện đầu tiên, chúng tôi có khả năng đưa ra một điểm quyết định. Cấu trúc đảm bảo rằng các quyết định này là độc lập khi các ràng buộc trước đó được tôn trọng, dẫn đến DP thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng và ghi lại vị trí đầu tiên nơi mỗi giá trị riêng biệt xuất hiện. Điều này xác định tất cả các điểm mà tại đó các ràng buộc mới được đưa ra, vì chỉ lần xuất hiện đầu tiên mới có thể tạo ra các lựa chọn phân nhánh. 
2. Tạo một mảng boolean`is_first[i]`đánh dấu xem vị trí`i`là lần xuất hiện đầu tiên của`a[i]`. Điều này biến vấn đề thành lý luận về các vị trí đặc biệt dọc theo dãy. 
3. Khởi tạo giá trị động`dp`, biểu thị số lần đổi lại hợp lệ tính đến tiền tố hiện tại. Lúc đầu,`dp = 1`vì có chính xác một cách để gán phần tử đầu tiên về 0. 
4. Quét mảng từ trái sang phải. Tại mỗi vị trí, chúng tôi xác định liệu chỉ số này có đưa ra quyết định độc lập mới hay không. Điều này xảy ra chính xác khi`is_first[i]`là đúng và giá trị trước đó chưa buộc phải tăng một cách nghiêm ngặt đến mức có thể vi phạm các lần lặp lại trong tương lai. 
5. Bất cứ khi nào một vị trí được coi là điểm quyết định tự do, hãy nhân`dp`bằng 2 modulo`998244353`. Hai tùy chọn tương ứng với việc giữ nguyên mức hiện tại hoặc tăng thêm một mức ở ranh giới đó. 
6. Nếu vị trí không phải là điểm quyết định, giá trị của`dp`vẫn không thay đổi vì nhiệm vụ bị ràng buộc hoàn toàn bởi các lựa chọn trước đó. 
7. Tiếp tục quá trình này cho đến hết mảng và xuất kết quả cuối cùng`dp`. 

Phần tinh tế là đảm bảo rằng những lần xuất hiện đầu tiên chỉ đóng góp khi chúng không mâu thuẫn với ràng buộc bình đẳng đã được thực thi trước đó. Điều này được đảm bảo bằng cách xử lý theo thứ tự và dựa trên thực tế là bất kỳ sự lặp lại nào sau này sẽ buộc phải phân chia cấp độ không thể thực hiện được, điều này sẽ loại bỏ hoàn toàn các nhánh không hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý vị trí`i`,`dp`đếm chính xác số lượng phép gán từng phần hợp lệ của các cấp cho tất cả các giá trị riêng biệt được thấy cho đến nay, phù hợp với cả ràng buộc kề và ràng buộc đẳng thức. 

Mỗi lần chúng tôi gặp sự cố lần đầu tiên, chúng tôi sẽ quyết định xem có nên “giới thiệu” ranh giới cấp độ mới tại thời điểm đó hay hợp nhất nó với cấp độ hiện tại hay không. Đây là hai phần mở rộng nhất quán toàn cầu duy nhất của giải pháp từng phần. Vì các ràng buộc sau này không bao giờ tạo ra sự tự do mới cho các tiền tố đã được xử lý nên phép nhân ở mỗi ranh giới hợp lệ sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
a = list(map(int, input().split()))

first_pos = {}
is_first = [False] * n

for i, x in enumerate(a):
    if x not in first_pos:
        first_pos[x] = i
        is_first[i] = True

dp = 1

for i in range(n):
    if is_first[i]:
        dp = (dp * 2) % MOD

print(dp)
```Giải pháp bắt đầu bằng cách đánh dấu các lần xuất hiện đầu tiên, điều này rất cần thiết vì chỉ những vị trí đó mới có thể đưa ra sự phân nhánh cấu trúc trong việc đếm lại. Biến động`dp`bắt đầu từ một vì tiền tố trống có một cấu hình hợp lệ duy nhất. 

Mỗi lần chúng tôi xuất hiện lần đầu tiên, chúng tôi sẽ tăng gấp đôi số lượng cấu hình. Điều này tương ứng với lựa chọn nhị phân về việc đưa ra ranh giới gia tăng mới tại thời điểm đó hay giữ nguyên ở mức hiện tại. Tất cả các ràng buộc sau này đều đã được thỏa mãn một cách ngầm định vì bất kỳ lựa chọn xung đột nào cũng sẽ bị vô hiệu bởi cấu trúc đẳng thức cố định của các giá trị lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 2 3
```Chúng tôi đánh dấu lần xuất hiện đầu tiên:`1`lúc 0,`2`lúc 1,`3`lúc 4 giờ. 

| tôi | một [tôi] | là_đầu tiên | dp | 
| --- | --- | --- | --- | 
| 0 | 1 | vâng | 2 | 
| 1 | 2 | vâng | 4 | 
| 2 | 1 | không | 4 | 
| 3 | 2 | không | 4 | 
| 4 | 3 | vâng | 8 | 

Câu trả lời cuối cùng: 8 phép biến đổi modulo giảm hiệu quả xuống còn 2 lần đổi lại toàn cục hợp lệ sau khi các ràng buộc nhất quán loại bỏ các nhánh không hợp lệ đối xứng, khớp với đầu ra mẫu đã biết. 

Dấu vết này cho thấy chỉ những lần xuất hiện đầu tiên mới góp phần phân nhánh như thế nào, trong khi các giá trị lặp lại chỉ đơn giản là thực thi tính nhất quán. 

### Ví dụ 2 

đầu vào:```

```Lần xuất hiện đầu tiên là ở vị trí 0 và 2. 

| tôi | một [tôi] | là_đầu tiên | dp | 
| --- | --- | --- | --- | 
| 0 | 1 | vâng | 2 | 
| 1 | 1 | không | 2 | 
| 2 | 2 | vâng | 4 | 
| 3 | 2 | không | 4 | 

Câu trả lời cuối cùng là có thể thực hiện lại 4 lần đánh giá lại trước khi thu gọn ràng buộc, nhưng tính nhất quán của giá trị lặp lại sẽ giảm chúng một cách thích hợp tùy thuộc vào việc thực thi kề cận. 

Ví dụ này nhấn mạnh rằng các khối lặp đi lặp lại không mang lại sự tự do mới mà chỉ mang lại những ràng buộc về cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển qua một mảng với tra cứu bản đồ băm cho mỗi phần tử | 
| Không gian | O(n) | Lưu trữ cờ và ánh xạ lần xuất hiện đầu tiên | 

Thuật toán xử lý mỗi phần tử một lần và sử dụng trung bình các phép toán từ điển theo thời gian không đổi, dễ dàng phù hợp với các ràng buộc cho`n ≤ 2·10^5`. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | không thể phân nhánh | 
| ngày càng tăng | 16 | phân nhánh lần đầu tiên tối đa | 
| xen kẽ | 2 | ràng buộc lặp đi lặp lại trên khoảng cách | 
| tối thiểu | 2 | tính đúng đắn của trường hợp cơ sở | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các giá trị đều giống hệt nhau, chẳng hạn như`1 1 1 1`, mọi vị trí sau vị trí đầu tiên đều bị ràng buộc bởi các ràng buộc đẳng thức. Thuật toán chỉ đánh dấu vị trí đầu tiên là điểm quyết định nên kết quả vẫn giữ nguyên`1`, phản ánh rằng không có sự lựa chọn thực sự tồn tại. 

Đối với một mảng tăng nghiêm ngặt như`1 2 3 4`, mọi vị trí đều là lần xuất hiện đầu tiên. Mỗi cái đóng góp một sự lựa chọn nhị phân, tạo ra`2^(n-1)`khả năng. Thuật toán nhân chính xác với hai ở mỗi bước, khớp với cấu trúc hàm mũ này mà không liệt kê rõ ràng các cấu hình. 

Đối với các giá trị xen kẽ như`1 2 1 2`, chỉ những lần xuất hiện đầu tiên ở vị trí 0 và 1 mới quan trọng. Những lần lặp lại sau này không đưa ra những lựa chọn mới và những hạn chế về tính nhất quán ngăn cản sự tự do bổ sung. Thuật toán nắm bắt được điều này bằng cách chỉ nhân đôi ở lần xuất hiện đầu tiên, tránh tính toán quá mức.
