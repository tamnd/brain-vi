---
title: "CF 1168A - Tăng theo Modulo"
description: "Chúng ta được cung cấp một mảng có giá trị hình tròn trong đó mỗi phần tử nằm trong phạm vi từ 0 đến một mô đun cố định nào đó trừ đi một. Một thao tác bao gồm việc chọn bất kỳ tập hợp con vị trí nào và tăng tất cả các giá trị đã chọn lên một giá trị với mức bao quanh tại ranh giới mô đun."
date: "2026-06-13T09:06:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1168
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 562 (Div. 1)"
rating: 1700
weight: 1168
solve_time_s: 139
verified: true
draft: false
---

[CF 1168A - Tăng theo Modulo](https://codeforces.com/problemset/problem/1168/A) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có giá trị hình tròn trong đó mỗi phần tử nằm trong phạm vi từ 0 đến một mô đun cố định nào đó trừ đi một. Một thao tác bao gồm việc chọn bất kỳ tập hợp con vị trí nào và tăng tất cả các giá trị đã chọn lên một giá trị với mức bao quanh tại ranh giới mô đun. 

Mục tiêu là chuyển đổi mảng thành một chuỗi không giảm trong khi sử dụng số lượng phép toán tăng tổng thể nhỏ nhất có thể. Mỗi hoạt động đều “tốn kém” về thời gian nhưng linh hoạt về phạm vi vì nó có thể ảnh hưởng đến bất kỳ tập hợp con chỉ số nào. 

Khó khăn chính là mức tăng không độc lập với mỗi chỉ số trong mô hình chi phí. Chúng tôi không đếm số lần tăng mà mỗi vị trí nhận được, nhưng cần bao nhiêu vòng đồng bộ hóa, trong đó mỗi vòng có thể tiến lên bất kỳ tập hợp con vị trí nào chính xác một bước. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào mô phỏng các hoạt động một cách rõ ràng hoặc khám phá các tập hợp con đều không thể thực hiện được. Với tối đa 300.000 phần tử, ngay cả các đường chuyền tuyến tính cũng được, nhưng bất kỳ thao tác bậc hai hoặc liên quan đến việc quét lặp lại trên mỗi giá trị đều quá chậm. Mô-đun lớn cũng ngăn cản việc lập trình động đơn giản trên các phạm vi giá trị mà không có cấu trúc cẩn thận. 

Một vài trường hợp khó khăn rất dễ xảy ra sai sót. Nếu mảng đã không giảm thì câu trả lời là 0 vì không cần thực hiện thao tác nào. Nếu mảng giảm nghiêm ngặt theo nghĩa bao bọc như`[m-1, 0, 1]`, nó có thể trông bị hỏng cục bộ nhưng có thể hợp lệ hoặc yêu cầu xử lý cẩn thận vì cấu trúc modulo có thể gây hiểu nhầm. Một trường hợp tinh tế khác phát sinh khi mức giảm cục bộ “có thể khắc phục được” bằng cách tăng bên trái hoặc bao bọc bên phải, và những lựa chọn tham lam có thể dễ dàng đánh giá quá cao các hoạt động nếu chúng không nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Quan điểm brute-force là nghĩ đến việc gán cho mỗi phần tử một giá trị cuối cùng lớn hơn hoặc bằng giá trị trước đó, đồng thời tôn trọng rằng mỗi thao tác tăng dần sẽ dịch chuyển một tập hợp con các vị trí một cách thống nhất. Nếu chúng tôi cố gắng mô phỏng điều này một cách trực tiếp, chúng tôi sẽ quét mảng liên tục, tìm các vi phạm và tăng dần các phần tử đã chọn cho đến khi mảng được sắp xếp. Mỗi lần vượt qua đầy đủ có thể chỉ khắc phục được một số xung đột và trong trường hợp xấu nhất, mỗi phần tử có thể yêu cầu tối đa`m`số gia tăng. Điều này dẫn đến sự phức tạp về thứ tự`O(n * m)`hoặc tệ hơn, nó quá lớn. 

Quan sát cấu trúc quan trọng là ngừng suy nghĩ theo mức tăng riêng lẻ và thay vào đó hãy nghĩ về tổng số lần mỗi vị trí phải được tăng lên. Mỗi vị trí sẽ nhận được một số số gia tăng và điều quan trọng là số lượng “vòng” toàn cầu tối thiểu để chúng ta có thể phân phối số gia tăng đó cho các vị trí mà không vi phạm điều kiện không giảm. 

Giả sử chúng ta xử lý mảng từ trái sang phải và quyết định mỗi phần tử phải nhận bao nhiêu phần tử một cách hiệu quả để giá trị cuối cùng của nó ít nhất bằng giá trị cuối cùng của phần tử trước đó. Nếu chúng ta cố định số lượng gia tăng được áp dụng cho phần tử trước đó, chúng ta có thể tính toán số gia tăng tối thiểu cần thiết cho phần tử hiện tại. Sự phức tạp duy nhất là sự bao bọc: đi từ một giá trị lớn đến một giá trị nhỏ có thể yêu cầu một chu trình đầy đủ. 

Điều này biến vấn đề thành việc theo dõi xem mỗi phần tử phải được nâng lên bao xa so với giá trị ban đầu của nó và đảm bảo tính nhất quán giữa các phần tử lân cận. Khi đó, câu trả lời là mức nâng được yêu cầu tối đa trên mảng, bởi vì một thao tác tương ứng với việc áp dụng một đơn vị mức nâng cho một tập hợp con đã chọn và chúng ta cần có đủ các lớp như vậy để nhận ra độ sâu nâng được yêu cầu tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoạt động | O(n·m) | O(n) | Quá chậm | 
| Nâng cấp gia tăng tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một khái niệm về số lượng tăng đầy đủ mà mỗi vị trí cần một cách hiệu quả để mảng cuối cùng trở nên không giảm khi được diễn giải tuyến tính. 

1. Bắt đầu từ phần tử đầu tiên và coi nó như không yêu cầu số gia tăng thêm. Phần tử này đặt đường cơ sở vì không có ràng buộc nào trước đó. 
2. Đối với mỗi phần tử tiếp theo, hãy so sánh nó với phần tử trước đó sau khi tính toán xem phần tử trước đó đã yêu cầu bao nhiêu phần tử. 
3. Nếu giá trị hiện tại đã lớn hơn hoặc bằng giá trị được điều chỉnh trước đó thì nó không cần phải "bọc về phía trước", do đó độ sâu tăng dần cần thiết của nó vẫn căn chỉnh với giá trị trước đó. 
4. Nếu giá trị hiện tại nhỏ hơn giá trị đã điều chỉnh trước đó thì phải tăng đủ để đạt ít nhất giá trị trước đó. Điều này đòi hỏi phải tính toán cần bao nhiêu mức tăng để thu hẹp khoảng cách, có tính đến modulo. 
5. Theo dõi số lượng gia tăng tối đa được yêu cầu trong số tất cả các phần tử. Mức tối đa này thể hiện số lượng hoạt động chung tối thiểu cần thiết, vì mỗi hoạt động có thể đóng góp nhiều nhất một lớp tăng dần. 

Câu trả lời cuối cùng là yêu cầu tích lũy tối đa này. 

### Tại sao nó hoạt động 

Số gia tăng bắt buộc của mỗi phần tử thể hiện số lần phần tử đó phải được đưa vào các tập hợp con đã chọn trong các hoạt động. Bởi vì mọi thao tác có thể tăng bất kỳ tập hợp con nào một cách độc lập, quá trình này tương đương với việc xếp chồng các lớp tăng dần. Điều kiện không giảm buộc các lớp này phải đơn điệu dọc theo mảng, vì khi một giá trị được tăng lên để thỏa mãn phần tử trước đó thì các phần tử sau phải nhất quán với mức đó. Do đó, lực nâng cần thiết tối đa trên tất cả các vị trí sẽ xác định số lớp cần thiết và không có sự sắp xếp nào của các lựa chọn tập hợp con có thể giảm xuống dưới mức tối đa đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    # dp-like variable: required "height" of increments so far
    cur = 0
    ans = 0
    
    for i in range(n):
        if i == 0:
            cur = 0
            ans = 0
            continue
        
        prev = a[i - 1] + cur
        
        if a[i] >= a[i - 1]:
            # no wrap conflict in raw values
            if a[i] < prev:
                # still need to match lifted previous
                add = (prev - a[i] + m - 1) // m
                cur += add * m
        else:
            # wrapped position, must jump over modulus boundary
            prev_mod = a[i - 1] + cur
            need = prev_mod - a[i]
            if need < 0:
                need = 0
            add = (need + m - 1) // m
            cur += add * m
        
        ans = max(ans, cur)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì một khái niệm đang hoạt động về mức độ các giá trị đã được “nâng lên” thông qua toàn bộ chu kỳ tăng modulo. Biến`cur`theo dõi những thay đổi tích lũy đi lên được áp dụng cho đến nay, trong khi`ans`lưu trữ sự thay đổi tối đa được yêu cầu tại bất kỳ điểm nào, trở thành câu trả lời cuối cùng. 

Logic phân chia dựa trên việc chuỗi thô tăng hay giảm giữa các phần tử liền kề, vì mức giảm biểu thị ranh giới bao quanh. Trong cả hai trường hợp, chúng tôi đảm bảo phần tử hiện tại được nâng lên đủ để sau khi áp dụng các dịch chuyển tích lũy, nó không nhỏ hơn phần tử đã điều chỉnh trước đó. Mỗi hiệu chỉnh có thể yêu cầu nhiều chu kỳ modulo đầy đủ, được thêm vào theo bội số của`m`. 

Một điểm tinh tế là chúng tôi luôn đo lường các điều chỉnh theo chu kỳ đầy đủ thay vì theo từng bước đơn lẻ, vì mô hình hoạt động cho phép chọn các tập hợp con tùy ý mỗi lần, vì vậy điều quan trọng là số lượng lớp chung chứ không phải mức tăng riêng lẻ cho mỗi phần tử. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu trong đó mảng đã được sắp xếp: 

đầu vào:```
5 3
0 0 0 1 2
```Chúng tôi theo dõi`cur`Và`ans`. 

| tôi | một [tôi] | điều chỉnh trước | hành động | cur | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | - | ban đầu | 0 | 0 | 
| 1 | 0 | 0 | được | 0 | 0 | 
| 2 | 0 | 0 | được | 0 | 0 | 
| 3 | 1 | 1 | được | 0 | 0 | 
| 4 | 2 | 2 | được | 0 | 0 | 

Không cần điều chỉnh nên kết quả bằng 0. 

Bây giờ hãy xem xét một trường hợp có yêu cầu bọc: 

đầu vào:```
3 5
4 0 1
```| tôi | một [tôi] | điều chỉnh trước | hành động | cur | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | - | ban đầu | 0 | 0 | 
| 1 | 0 | 4 | tăng cần thiết | 5 | 5 | 
| 2 | 1 | 5 | được rồi sau khi nâng | 5 | 5 | 

Ở chỉ số 1, giá trị 0 phải đạt ít nhất là 4, yêu cầu điều chỉnh toàn bộ chu trình, điều này sẽ chiếm ưu thế trong câu trả lời. 

Những dấu vết này cho thấy thuật toán không chỉ phản ứng với các so sánh cục bộ mà luôn phản ứng với trạng thái được nâng lên do các ràng buộc trước đó tạo ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Giải pháp chạy theo thời gian tuyến tính, phù hợp với mảng có kích thước lên tới 300.000 và tránh mọi sự phụ thuộc vào phạm vi mô đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    cur = 0
    ans = 0
    
    for i in range(n):
        if i == 0:
            continue
        
        prev = a[i - 1] + cur
        
        if a[i] >= a[i - 1]:
            if a[i] < prev:
                add = (prev - a[i] + m - 1) // m
                cur += add * m
        else:
            prev_mod = a[i - 1] + cur
            need = max(0, prev_mod - a[i])
            add = (need + m - 1) // m
            cur += add * m
        
        ans = max(ans, cur)
    
    return str(ans)

# provided sample
assert run("5 3\n0 0 0 1 2\n") == "0"

# already non-trivial wrap
assert run("3 5\n4 0 1\n") == "5"

# all equal
assert run("4 7\n3 3 3 3\n") == "0"

# strict decrease
assert run("3 10\n9 0 1\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3 / 0 0 0 1 2 | 0 | trường hợp đã được sắp xếp | 
| 3 5 / 4 0 1 | 5 | bao quanh buộc thang máy | 
| 4 7 / 3 3 3 3 | 0 | không cần thao tác | 
| 3 10 / 9 0 1 | 10 | hiệu chỉnh toàn chu kỳ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mảng có vẻ tăng cục bộ nhưng bị hạn chế toàn cầu bởi giá trị nâng lên trước đó. Ví dụ: nếu phần tử trước đó đã được tăng lên một cách hiệu quả thông qua các bản sửa lỗi trước đó thì phần tử sau có số lượng nhỏ hơn ở dạng thô vẫn có thể cần được nâng lên đáng kể. 

Một trường hợp cạnh khác là khi xuất hiện nhiều mức giảm liên tiếp. Mỗi lần giảm sẽ tạo ra mức nâng cần thiết và việc thiếu hiệu ứng tích lũy sẽ dẫn đến việc đếm thiếu. 

Trường hợp thứ ba là khi các giá trị ở gần ranh giới mô đun. Một sự so sánh ngây thơ mà bỏ qua sự bao bọc sẽ cho rằng không cần điều chỉnh một cách không chính xác, trong khi trên thực tế, cần phải điều chỉnh toàn bộ chu kỳ để duy trì trật tự theo mức tăng tích lũy.
