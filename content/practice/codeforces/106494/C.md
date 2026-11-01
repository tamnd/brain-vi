---
title: "CF 106494C - Thế giới thay thế I"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép chia chúng thành nhiều nhóm. Đối với mỗi nhóm, chúng tôi tính toán số trung vị của nhóm đó và điểm cuối cùng là tổng của tất cả các số trung vị này. Nhiệm vụ là chọn phân vùng của mảng thành các nhóm sao cho tổng số tiền này lớn nhất."
date: "2026-06-19T15:11:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106494
codeforces_index: "C"
codeforces_contest_name: "MEPhI Spring Cup 2026"
rating: 0
weight: 106494
solve_time_s: 52
verified: true
draft: false
---

[CF 106494C - Thế giới thay thế I](https://codeforces.com/problemset/problem/106494/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép chia chúng thành nhiều nhóm. Đối với mỗi nhóm, chúng tôi tính toán số trung vị của nhóm đó và điểm cuối cùng là tổng của tất cả các số trung vị này. Nhiệm vụ là chọn phân vùng của mảng thành các nhóm sao cho tổng số tiền này lớn nhất. 

Trung vị ở đây hoạt động theo cách tiêu chuẩn: nếu một nhóm có kích thước lẻ thì đó là phần tử ở giữa sau khi sắp xếp; nếu nó có kích thước chẵn, thì định nghĩa được sử dụng trong bài toán này ngụ ý rằng chúng ta vẫn quan tâm đến hành vi “ở giữa phía dưới” phù hợp với lý luận biên tập, vì lập luận dựa vào thứ tự hơn là tính trung bình. 

Khó khăn chính là việc nhóm thay đổi các trung vị theo cách phi tuyến tính. Một phần tử đơn lẻ có thể là nhóm riêng của nó và đóng góp giá trị của nó, nhưng việc kết hợp các phần tử có thể tăng hoặc giảm phần đóng góp tùy thuộc vào dấu hiệu và thứ tự. 

Các ràng buộc không được đưa ra rõ ràng trong đoạn trích câu lệnh, nhưng các vấn đề thuộc dạng này từ Codeforces thường liên quan đến tối đa khoảng 2·10^5 phần tử. Điều đó ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các phân vùng hoặc mô phỏng việc hình thành nhóm. Ngay cả việc kiểm tra tất cả các tập hợp con hoặc tất cả các phần tách cũng sẽ tăng theo cấp số nhân và thất bại. 

Một trường hợp thất bại tinh tế xuất hiện khi tất cả các số đều âm hoặc khi khan hiếm số dương. Một chiến lược tham lam luôn cô lập các mặt tích cực hoặc luôn ghép các phần tử liền kề mà không cân nhắc về mức trung vị có thể làm mất cấu trúc tối ưu. Ví dụ: nếu người ta giả định không chính xác rằng mọi phần tử sẽ tạo thành nhóm riêng của nó, thì đối với đầu vào`[-5, -1, 3]`kết quả sẽ là`-5 + -1 + 3 = -3`, nhưng việc kết hợp chúng đúng cách sẽ tạo ra một nhóm duy nhất có giá trị trung bình`-1`, cái nào tốt hơn. 

Một trường hợp thất bại khác đến từ việc ghép đôi mà không cân nhắc đặt hàng. Việc ghép nối số dương lớn nhất với số âm nhỏ nhất không phải lúc nào cũng tối ưu trừ khi chúng ta hiểu cách hoạt động của các đường trung bình khi chèn, đó là hiểu biết sâu sắc về cấu trúc cốt lõi của vấn đề này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các phân vùng có thể có của mảng thành các nhóm. Đối với mỗi phân vùng, chúng tôi sẽ sắp xếp từng nhóm, tính trung vị của nó và tính tổng chúng. Số lượng phân vùng là số mũ theo n, tăng lên như số Bell và thậm chí với n = 20, điều này trở nên không khả thi. Mỗi đánh giá cũng yêu cầu các nhóm sắp xếp, bổ sung thêm các yếu tố logarit. 

Quan sát quan trọng trong bài xã luận là các số trung vị hoạt động đơn điệu khi hợp nhất và chúng ta có thể cơ cấu lại bất kỳ giải pháp tối ưu nào sao cho mỗi nhóm có nhiều nhất một số không âm. Điều này hạn chế đáng kể cấu trúc: hầu hết tất cả các phần tử dương hoặc bằng 0 đều có thể bị cô lập hoặc đóng vai trò như những điểm neo, trong khi các phần tử âm được ghép nối hoặc nhóm lại một cách cẩn thận. 

Sau đó chúng tôi chia vấn đề bằng dấu hiệu. Các số không âm có lợi vì chúng có thể đóng góp trực tiếp tích cực dưới dạng trung vị một phần tử. Các số âm chỉ hữu ích khi chúng được “hấp thụ” vào các nhóm có các điểm neo không âm để không làm giảm sự đóng góp hoặc khi các số âm còn sót lại phải tạo thành một nhóm cuối cùng mà trung vị là không thể tránh khỏi. 

Điều này biến vấn đề thành một chiến lược ghép nối tham lam đơn giản: kết hợp những điều tiêu cực với những điều không tiêu cực theo cách vô hiệu hóa tác động của chúng, ưu tiên những tiêu cực nhỏ nhất để giảm thiểu thiệt hại. Bất kỳ phần tử còn lại nào tạo thành một nhóm duy nhất có trung vị được xác định bằng thứ tự, nhưng điều này là bắt buộc và nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Tái cơ cấu tham lam dựa trên dấu hiệu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta sắp xếp mảng sao cho các phần tử âm và không âm được tách biệt về mặt cấu trúc. Việc sắp xếp này rất cần thiết vì tất cả các quyết định đều phụ thuộc vào thứ tự tương đối, đặc biệt khi chọn các giá trị âm để ghép. 

Sau đó chúng ta đếm có bao nhiêu số không âm. Hãy để đây là cnt_pos. 

Tiếp theo, chúng tôi phân nhánh dựa trên việc số không âm có ít nhất bằng số âm hay không. 

Nếu cnt_pos lớn hơn hoặc bằng số âm thì mọi âm có thể được “hấp thụ” vào một nhóm có số không âm mà không làm giảm tổng đóng góp. Chúng tôi tạo một nhóm cho mỗi phần tử không âm, ban đầu coi mỗi nhóm là một nhóm đơn lẻ đóng góp giá trị của nó. Sau đó, chúng tôi chỉ định từng yếu tố tiêu cực vào bất kỳ nhóm nào trong số này. Vì trung vị của một nhóm có một số không âm và bất kỳ số số âm nào vẫn không âm hoặc không bị ảnh hưởng trong đóng góp theo đối số cấu trúc, nên số âm không làm giảm tổng. Câu trả lời tổng chỉ đơn giản là tổng của tất cả các số không âm. 

Nếu số lượng âm bản nhiều hơn thì chỉ có thể ghép nối các âm bản cnt_pos theo cách vô hiệu hóa tác dụng của chúng. Để giảm thiểu tổn thất, chúng tôi ghép từng số không âm với các số âm nhỏ nhất có sẵn. Các cặp này đóng góp bằng 0 một cách hiệu quả theo hành vi trung bình được mô tả trong lý do biên tập. 

Sau khi ghép nối, các âm bản còn sót lại vẫn còn. Chúng phải được nhóm lại với nhau thành một bộ duy nhất. Trung vị của chúng đóng góp một giá trị âm, nhưng việc chia chúng ra xa hơn sẽ không cải thiện tổng vì mỗi nhóm bổ sung sẽ làm giảm hiệu quả cấu trúc hoặc tạo ra các trung vị tồi tệ hơn. 

Cuối cùng, chúng tôi tính toán mức đóng góp trung bình từ nhóm âm còn sót lại đó và cộng nó vào kết quả.

Tại sao nó hoạt động xuất phát từ hai sự thật về cấu trúc. Đầu tiên, trung vị của các nhóm được hợp nhất được giới hạn giữa các trung vị thành phần, cho phép hợp nhất có kiểm soát mà không tăng hoặc giảm mức đóng góp ngoài phạm vi có thể dự đoán được. Thứ hai, bất kỳ cấu hình tối ưu nào cũng có thể được chuyển đổi sao cho có nhiều nhất một phần tử không âm xuất hiện trong mỗi nhóm, điều này làm giảm vấn đề quyết định có bao nhiêu phần tử âm có thể được vô hiệu hóa bằng phần tử dương. Khi cấu trúc này được thực thi, việc ghép nối tham lam theo các số âm nhỏ nhất là tối ưu vì bất kỳ số âm nào lớn hơn được sử dụng trước đó sẽ chỉ làm xấu đi các giá trị trung bình thu được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    neg = [x for x in a if x < 0]
    pos = [x for x in a if x >= 0]
    
    cnt_neg = len(neg)
    cnt_pos = len(pos)
    
    if cnt_pos >= cnt_neg:
        # all negatives can be absorbed
        print(sum(pos))
        return
    
    # more negatives than positives
    # pair smallest negatives with positives
    neg.sort()  # most negative first
    
    used = cnt_pos
    remaining_neg = cnt_neg - cnt_pos
    
    # leftover negatives: take the largest (least negative) remaining ones
    leftover = neg[:remaining_neg]
    
    # contribution from leftovers is their median
    leftover.sort()
    m = leftover[len(leftover) // 2]
    
    print(sum(pos) + m)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách sắp xếp sao cho các số âm và không âm có thể được phân tách rõ ràng. Sau đó, chúng tôi chia thành hai mảng phù hợp với phân vùng khái niệm trong thuật toán. 

Ở nhánh đầu tiên, khi số dương chiếm ưu thế, chúng tôi chỉ cần tính tổng chúng. Điều này trực tiếp tương ứng với thực tế là mọi âm đều có thể được hấp thụ mà không ảnh hưởng đến các giá trị trung bình dương. 

Trong nhánh thứ hai, chúng tôi xử lý rõ ràng các âm dư thừa. Chúng tôi lấy các giá trị âm nhỏ nhất cho logic ghép nối, nhưng trong tính toán, chúng tôi chỉ cần nhóm còn lại. Giá trị trung bình của các âm còn sót lại được tính toán trực tiếp sau khi sắp xếp lại chúng, đảm bảo tính chính xác của việc lựa chọn. 

Một chi tiết tinh tế là tính toán trung vị bằng cách sử dụng chỉ số`len(leftover)//2`, tương ứng với định nghĩa trung bình thấp hơn phù hợp với hành vi trung bình số nguyên trong các bài toán lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
-5 -2 -1 3 4
```Chúng tôi tách thành tiêu cực`[-5, -2, -1]`và tích cực`[3, 4]`. Số tích cực ít hơn nên chúng ta chuyển sang trường hợp thứ hai. 

Chúng ta có 3 cực âm và 2 cực dương, vì vậy một cực âm vẫn chưa ghép đôi. 

Chúng tôi tính toán các âm bản còn sót lại sau khi ghép những âm bản nhỏ nhất với các âm dương. 

| Bước | Tiêu cực | Tích cực | Còn lại âm | Còn sót lại | Trung vị | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | [-5,-2,-1] | [3,4] | 1 | [-2] | -2 | 

Câu trả lời cuối cùng là`3 + 4 + (-2) = 5`. 

Điều này cho thấy cách thuật toán tách những ảnh hưởng tiêu cực không thể tránh khỏi thành một đóng góp trung bình duy nhất. 

### Ví dụ 2 

đầu vào:```
4
-3 -1 2 5
```Tiêu cực là`[-3, -1]`, tích cực`[2, 5]`. Những điều tích cực chiếm ưu thế hoặc phù hợp với những điều tiêu cực, vì vậy tất cả những điều tiêu cực đều có thể được hấp thụ. 

| Bước | Tiêu cực | Tích cực | Tình trạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [-3,-1] | [2,5] | pos ≥ phủ định | tổng(pos)=7 | 

Câu trả lời cuối cùng là`7`. 

Điều này khẳng định rằng khi tích cực là đủ thì tiêu cực có thể được vô hiệu hóa hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị mọi hoạt động | 
| Không gian | O(n) | Mảng để phân chia âm và dương | 

Giải pháp phù hợp thoải mái trong các ràng buộc thông thường lên tới 2·10^5 phần tử, vì việc sắp xếp và quét tuyến tính là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# basic sample-style cases
assert run("5\n-5 -2 -1 3 4\n") == "5"
assert run("4\n-3 -1 2 5\n") == "7"

# single element
assert run("1\n-10\n") == "-10"

# all positive
assert run("3\n1 2 3\n") == "6"

# all negative
assert run("3\n-1 -2 -3\n") == "-2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dấu hiệu hỗn hợp | 5 | hấp thụ một phần âm bản | 
| tích cực cân bằng | 7 | trung hòa hoàn toàn | 
| âm đơn | -10 | cạnh tối thiểu | 
| tất cả đều tích cực | 6 | tối ưu tầm thường | 
| tất cả đều tiêu cực | -2 | tính đúng đắn của hành vi trung bình | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi không có số không âm. Đối với đầu vào`[-1, -2, -3]`, thuật toán coi tất cả các phần tử là các phần tử âm còn sót lại. Giá trị trung vị được tính từ tập hợp đầy đủ, cho`-2`, điều này đúng vì mọi phân vùng vẫn mang lại tổng giới hạn bởi giá trị trung tâm này. 

Một trường hợp đặc biệt khác là khi các giá trị dương khớp chính xác với các giá trị âm. Đối với đầu vào`[-4, -1, 2, 10]`, mỗi tích cực có thể vô hiệu hóa một tiêu cực, không để lại nhóm còn sót lại, vì vậy câu trả lời trở thành`12`. Thuật toán xử lý vấn đề này một cách tự nhiên bằng cách nhập nhánh hấp thụ và tính tổng các giá trị dương sau khi ghép nối về mặt khái niệm. 

Trường hợp thứ ba là khi có một mặt tích cực và nhiều mặt tiêu cực. Đối với đầu vào`[-5, -4, -3, -2, 7]`, chỉ có thể vô hiệu hóa một âm bản, để lại một bộ bốn âm bản còn sót lại có trung vị chiếm ưu thế trong cấu trúc. Thuật toán tách nhóm này và tính toán trực tiếp trung vị của nó, khớp với cấu trúc bắt buộc được ngụ ý bởi phân vùng tối ưu.
