---
title: "CF 105941A - Toxel \u4e0e\u72ec\u4e00\u65e0\u4e8c\u7684\u5e8f\u5217"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một mảng có độ dài $n$, trong đó mọi phần tử là một số nguyên nằm trong khoảng từ $1$ đến $n$."
date: "2026-06-22T15:51:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105941
codeforces_index: "A"
codeforces_contest_name: "2025 National Invitational of CCPC (Zhengzhou), 2025 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105941
solve_time_s: 100
verified: true
draft: false
---

[CF 105941A - Toxel \u4e0e\u72ec\u4e00\u65e0\u4e8c\u7684\u5e8f\u5217](https://codeforces.com/problemset/problem/105941/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một mảng có độ dài$n$, trong đó mỗi phần tử là một số nguyên nằm giữa$1$Và$n$. Mục tiêu là biến mảng này thành một hoán vị của$1 \ldots n$, nghĩa là mọi số từ$1$ĐẾN$n$xuất hiện đúng một lần trong mảng cuối cùng. 

Hoạt động duy nhất được phép là chọn một đoạn liền kề$[l, r]$và ghi đè lên mọi vị trí trong phân đoạn đó. Mỗi vị trí bên trong phân đoạn có thể được gán bất kỳ giá trị nào từ$1$ĐẾN$n$, độc lập với các thao tác khác, do đó, một thao tác đơn lẻ có thể thiết kế lại hoàn toàn một khối của mảng. 

Nhiệm vụ là xác định số lượng tối thiểu các thao tác phân đoạn như vậy cần thiết để biến mảng ban đầu thành một số hoán vị hợp lệ. 

Quan sát quan trọng là chúng ta không bắt buộc phải bảo toàn bất kỳ cấu trúc nào từ mảng ban đầu mà chỉ đạt đến trạng thái cuối cùng là hoán vị. Vì các giá trị có thể được gán tự do bên trong một phân đoạn được vận hành nên vấn đề giảm xuống còn việc quyết định vị trí nào chúng ta chọn để cố định trong một khối vận hành tại một thời điểm. 

Các hạn chế là nhỏ:$n \le 40$, và nhiều nhất$10^4$trường hợp thử nghiệm. Điều này cho thấy rõ ràng rằng chúng tôi không mong đợi mô phỏng bất cứ điều gì theo cấp số nhân cho mỗi trường hợp thử nghiệm. Thay vào đó, cấu trúc của câu trả lời cho mỗi trường hợp kiểm thử phải có thể trích xuất được theo thời gian tuyến tính hoặc gần tuyến tính. 

Một cách giải thích ngây thơ sẽ là cố gắng xây dựng hoán vị cuối cùng một cách rõ ràng hoặc cố gắng sửa từng điểm không khớp một cách tham lam. Điều đó sẽ an toàn nhưng phức tạp không cần thiết. 

Một điểm tinh tế là có thể tồn tại nhiều chiến lược tối ưu vì mọi hoán vị đều có thể chấp nhận được. Điều này có nghĩa là chúng ta có thể tự do hướng tới một hoán vị kinh điển đơn giản hơn là lý luận về việc sắp xếp lại các giá trị một cách tùy ý. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng việc xây dựng hoán vị cuối cùng từng bước. Ở mỗi bước, chúng ta có thể chọn bất kỳ phân đoạn nào và gán giá trị để loại bỏ dần xung đột trong mảng. Tuy nhiên, do mỗi thao tác ảnh hưởng đến toàn bộ khoảng và việc lựa chọn phép gán không bị ràng buộc nên điều này dẫn đến hệ số phân nhánh rất lớn. Ngay cả đối với$n = 40$, việc khám phá tất cả các chuỗi phân đoạn có thể là hoàn toàn không khả thi. 

Sự đơn giản hóa quan trọng đến từ việc thay đổi quan điểm: thay vì suy nghĩ về giá trị nào chúng ta muốn trong hoán vị cuối cùng, chúng ta sửa một hoán vị mục tiêu cụ thể. Sự lựa chọn thuận tiện nhất là hoán vị danh tính$[1, 2, \ldots, n]$. Vì bất kỳ hoán vị nào cũng hợp lệ, nếu chúng ta có thể chuyển đổi mảng thành danh tính thì chúng ta đã hoàn thành. 

Bây giờ vấn đề trở nên thuần túy về vị trí. Chúng tôi xem xét mọi chỉ số$i$. Nếu như$a_i = i$, vị trí đó đã khớp với mục tiêu và không cần thực hiện bất kỳ thao tác nào. Nếu như$a_i \neq i$, sau đó định vị$i$phải được bao gồm trong ít nhất một hoạt động. 

Vì mỗi thao tác bao gồm một phân đoạn liền kề nên chi phí sẽ trở thành số lượng các phân đoạn liền kề được hình thành bởi các chỉ số cần sửa. Mỗi phân đoạn như vậy có thể được sửa trong một thao tác bằng cách ghi đè chính xác phạm vi đó. 

Điều này làm giảm vấn đề xác định các khối chỉ số liền kề tối đa trong đó$a_i \neq i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các hoạt động phân khúc | Hàm mũ | O(n) | Quá chậm | 
| Đếm các khối không khớp liền kề | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng ta đọc mảng và so sánh khái niệm từng vị trí$i$với chỉ số của nó. Nếu giá trị đã bằng$i$, chúng tôi coi nó như đã đúng và không làm gì với nó. 
2. Chúng tôi quét từ trái sang phải và đánh dấu xem vị trí hiện tại có “xấu” hay không, nghĩa là$a_i \neq i$. Sự phân loại này là thông tin duy nhất cần thiết để tính toán câu trả lời. 
3. Chúng tôi đếm số lần một khối vị trí xấu liền kề mới bắt đầu. Một khối bắt đầu ở vị trí$i$nếu như$a_i \neq i$và một trong hai$i = 1$hoặc$a_{i-1} = i-1$. 
4. Mỗi khối như vậy tương ứng với chính xác một thao tác, bởi vì một thao tác ngắt quãng có thể vẽ lại toàn bộ phạm vi liền kề của các vị trí không chính xác. 
5. Chúng tôi xuất ra số khối này làm câu trả lời cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mọi thao tác chỉ có thể sửa đổi một phân đoạn liền kề, do đó, bất kỳ tập hợp vị trí nào chúng ta cố định cùng nhau đều phải nằm trong một khoảng duy nhất. Ngược lại, bất kỳ đoạn liền kề tối đa nào có vị trí không chính xác luôn có thể được sửa trong một thao tác bằng cách chọn chính xác khoảng đó. Sẽ không có lợi ích gì khi chia một phân đoạn xấu thành nhiều hoạt động, vì mỗi lần phân chia sẽ chỉ tăng số lượng khoảng thời gian mà không cải thiện tính khả thi. 

Kết quả rút ra từ thực tế là chi phí chính xác là số lượng thành phần được kết nối trong tập hợp các chỉ số khác với giá trị mục tiêu của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        i = 0
        while i < n:
            if a[i] == i + 1:
                i += 1
                continue
            
            ans += 1
            j = i
            while j < n and a[j] != j + 1:
                j += 1
            i = j
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng quét mảng một lần cho mỗi trường hợp thử nghiệm. Khi tìm thấy một khối không khớp, nó sẽ mở rộng sang bên phải cho đến khi khối không khớp kết thúc và tính đó là một thao tác. Con trỏ sau đó nhảy qua khối, đảm bảo độ phức tạp tuyến tính. 

Một cạm bẫy phổ biến là lập chỉ mục theo từng cái một: vấn đề dựa trên 1, trong khi Python sử dụng lập chỉ mục dựa trên 0, vì vậy các phép so sánh sử dụng$a[i] == i + 1$. Một điểm tinh tế khác là chúng ta phải bỏ qua toàn bộ khối cùng một lúc; tăng từng cái một bên trong một khối vẫn có tác dụng nhưng sẽ dài dòng không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 8
a = [1, 1, 2, 1, 4, 1, 1, 2]
```Chúng tôi theo dõi các phân đoạn không khớp: 

| tôi | một [tôi] | tôi+1 | không khớp | khối hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không | - | 
| 2 | 1 | 2 | vâng | bắt đầu | 
| 3 | 2 | 3 | vâng | tiếp tục | 
| 4 | 1 | 4 | vâng | tiếp tục | 
| 5 | 4 | 5 | vâng | tiếp tục | 
| 6 | 1 | 6 | vâng | tiếp tục | 
| 7 | 1 | 7 | vâng | tiếp tục | 
| 8 | 2 | 8 | vâng | tiếp tục | 

Có một khối không khớp liền kề từ chỉ số 2 đến 8, vì vậy câu trả lời là 1 thao tác. 

Điều này chứng tỏ rằng ngay cả những phân đoạn hỗn loạn lớn cũng sẽ sụp đổ thành một thao tác sơn lại duy nhất khi chúng tiếp giáp nhau. 

### Ví dụ 2 

đầu vào:```
n = 6
a = [1, 2, 3, 4, 5, 6]
```Tất cả các vị trí đã phù hợp với mục tiêu của họ. 

| tôi | một [tôi] | tôi+1 | không khớp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | không | 
| 2 | 2 | 2 | không | 
| 3 | 3 | 3 | không | 
| 4 | 4 | 4 | không | 
| 5 | 5 | 5 | không | 
| 6 | 6 | 6 | không | 

Không có khối không khớp nào tồn tại nên câu trả lời là 0. 

Điều này xác nhận rằng các hoán vị đã đúng không cần thực hiện thao tác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Quét tuyến tính đơn để xác định các phân đoạn không khớp | 
| Không gian |$O(1)$thêm | Chỉ có bộ đếm và lưu trữ mảng đầu vào | 

Các ràng buộc cho phép lên đến$10^4$các trường hợp thử nghiệm, nhưng vì mỗi mảng có độ dài tối đa là 40 nên tổng công việc vẫn nhỏ ngay cả khi tải đầy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        i = 0
        while i < n:
            if a[i] == i + 1:
                i += 1
                continue
            ans += 1
            j = i
            while j < n and a[j] != j + 1:
                j += 1
            i = j
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided sample (format not fully specified, kept minimal sanity structure)
assert run("1\n8\n1 1 2 1 4 1 1 2\n") == "1"
assert run("1\n6\n1 2 3 4 5 6\n") == "0"

# custom cases
assert run("1\n1\n1\n") == "0"
assert run("1\n1\n2\n") == "1"
assert run("1\n5\n2 1 3 5 4\n") == "2"
assert run("2\n3\n1 2 3\n3\n2 3 1\n") == "0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đúng duy nhất | 0 | kích thước tối thiểu, đã hợp lệ | 
| yếu tố sai duy nhất | 1 | trường hợp hiệu chỉnh nhỏ nhất | 
| mảng hỗn hợp nhỏ | 2 | nhiều khối không khớp | 
| trộn hai trường hợp thử nghiệm | 0 / 1 | xử lý nhiều trường hợp | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp như$[1,2,\ldots,n]$, quá trình quét không tìm thấy khối không khớp và trực tiếp xuất ra số 0 vì không cần thao tác nào. 

Đối với một mảng hoàn toàn không chính xác như$[2,3,4,\ldots,1]$, mọi vị trí ban đầu đều xấu nhưng tạo thành một khối liền kề duy nhất, do đó thuật toán nén chính xác nó thành một thao tác. 

Đối với các mẫu chính xác xen kẽ như$[1,0,3,0,5,0]$được điều chỉnh theo các giới hạn hợp lệ, thuật toán sẽ đếm từng vùng xấu riêng biệt một cách độc lập, phản ánh rằng mỗi vùng yêu cầu một thao tác khoảng thời gian riêng biệt.
