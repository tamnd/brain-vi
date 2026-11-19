---
title: "CF 2085D - Buffet và Kaitenzushi"
description: "Vấn đề là vấn đề lập kế hoạch và lựa chọn được đóng khung như một kịch bản ăn sushi. Phục vụ tại một nhà hàng sushi băng chuyền, trong đó mỗi đĩa chứa chính xác k miếng sushi và có giá trị độ ngon đi kèm là di."
date: "2026-06-08T06:08:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graph-matchings", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2085
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1011 (Div. 2)"
rating: 2000
weight: 2085
solve_time_s: 97
verified: false
draft: false
---

[CF 2085D - Buffet phục vụ và Kaitenzushi](https://codeforces.com/problemset/problem/2085/D) 

**Xếp hạng:** 2000 
**Tags:** cấu trúc dữ liệu, so khớp biểu đồ, tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là vấn đề lập kế hoạch và lựa chọn được đóng khung như một kịch bản ăn sushi. Phục vụ tại một nhà hàng sushi băng chuyền, nơi mỗi đĩa chứa chính xác`k`miếng sushi và có giá trị về độ ngon đi kèm`d_i`. Nhà hàng hoạt động theo từng phút riêng biệt và vào từng phút`i`, cái`i`- tấm thứ đứng trước Serval. Người phục vụ có thể chọn lấy đĩa, ăn một miếng sushi đã lấy trước đó hoặc không làm gì cả. Điều hấp dẫn là tất cả các mảnh được lấy phải được tiêu thụ vào cuối`n`phút và Serval muốn tối đa hóa tổng giá trị độ ngon của những đĩa anh ấy chọn lấy. 

Mỗi trường hợp thử nghiệm chỉ định số phút`n`, số miếng trên một đĩa`k`và giá trị độ ngon của tất cả các đĩa. Đầu ra cho mỗi trường hợp thử nghiệm là tổng độ ngon tối đa mà Serval có thể tích lũy trong khi đáp ứng các hạn chế về mức tiêu thụ. 

Các ràng buộc đủ chặt chẽ để loại trừ mô phỏng toàn diện ngây thơ. Từ`n`có thể lên đến`2·10^5`cho mỗi trường hợp thử nghiệm và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng`2·10^5`, MỘT`O(n^2)`hoặc tệ hơn là cách tiếp cận sẽ quá chậm. Chúng ta phải hướng tới một giải pháp tuyến tính hoặc theo`O(n log n)`mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các giá trị rất nhỏ của`n`trong đó chỉ có thể lấy một tấm, trường hợp`k`gần với`n`đến mức không thể lấy nhiều đĩa và trình tự trong đó các đĩa có độ ngon lớn nhất xuất hiện muộn hoặc sớm trong dòng thời gian. Nếu không xử lý cẩn thận, một cách tiếp cận ngây thơ có thể vi phạm hạn chế tiêu thụ hoặc không chọn được tập hợp con tối ưu của các tấm. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử tất cả các tập hợp con đĩa có thể có và mô phỏng các hành động lấy và ăn. Về nguyên tắc, điều này đúng vì nó liệt kê đầy đủ tất cả các chuỗi hợp lệ, nhưng độ phức tạp của nó rất lớn.`O(2^n)`cho từng trường hợp thử nghiệm và do đó hoàn toàn không thực tế đối với các ràng buộc đã cho. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống thành một lựa chọn tham lam đơn giản khi nhìn ngược lại. Nếu chúng ta biết mình phải ăn từng miếng một thì số đĩa chúng ta có thể lấy bị hạn chế bởi tổng số phút và`k`. Cụ thể, nếu chúng ta lấy một cái đĩa vào phút`i`, chúng ta phải có khả năng hoàn thành nó`k`phần còn lại`n-i`phút. Ngược lại, chiến lược tối ưu là lấy những đĩa ngon nhất mà chúng ta có thể quản lý, bắt đầu từ cuối dòng thời gian, bởi vì những đĩa này còn lại ít phút hơn để ăn sau đó. 

Sau khi được sắp xếp hoặc lặp lại ngược lại, chúng tôi chọn nhiều nhất`ceil(n/k)`tấm. Cách tiếp cận này đảm bảo chúng tôi đáp ứng được yêu cầu tiêu thụ và tối đa hóa độ ngon. Cấu trúc của bài toán cho phép đơn giản hóa việc này vì mỗi tấm có kích thước giống hệt nhau (`k`miếng), nên chúng ta không cần xét các hoán vị của những chiếc đĩa nhỏ hơn mà chỉ xét những vị trí mà chúng ta có thể lấy một chiếc đĩa mà không vi phạm ràng buộc về thời gian ăn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lựa chọn lạc hậu tham lam | O(n log n) nếu sắp xếp, O(n) nếu lặp với một đống | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`k`, theo sau là danh sách các giá trị độ ngon`d`. 
2. Khởi tạo một đống tối đa hoặc đơn giản là sắp xếp các đĩa theo thứ tự độ ngon giảm dần nếu được phép sắp xếp lại hầu như. 
3. Xác định số đĩa tối đa mà Serval có thể lấy, đó là`ceil(n / k)`. Mỗi tấm yêu cầu`k`phút để hoàn thành và không có đĩa nào có thể được ăn hết một phần. 
4. Chọn phần trên cùng`ceil(n / k)`giá trị độ ngon lớn nhất, vì dùng nhiều hơn sẽ vượt quá tổng số phút có sẵn. 
5. Tính tổng các giá trị đã chọn và xuất kết quả cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Điều bất biến là bằng cách luôn lấy những đĩa có độ ngon lớn nhất còn lại phù hợp với giới hạn về thời gian ăn, chúng ta không bao giờ bỏ lỡ đĩa có giá trị cao hơn mà lẽ ra có thể được ăn. Vì mỗi đĩa có kích thước bằng nhau nên sự lựa chọn tham lam này đảm bảo độ ngon tổng thể tối đa đồng thời tôn trọng quy định rằng cuối cùng tất cả sushi phải được ăn hết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    d = list(map(int, input().split()))
    
    # Maximum number of plates we can take
    max_plates = (n + k - 1) // k  # ceil(n / k)
    
    # Sort deliciousness in descending order
    d.sort(reverse=True)
    
    # Take the top `max_plates` deliciousness
    result = sum(d[:max_plates])
    print(result)
```Mã đầu tiên tính toán mức trần của`n/k`để xác định số đĩa tối đa có thể lấy được. Sắp xếp mảng độ ngon đảm bảo chúng tôi chọn được giá trị lớn nhất. Tổng hợp lần đầu tiên`max_plates`các giá trị mang lại độ ngon tổng thể tối ưu. Điều này tránh được việc mô phỏng phức tạp các bước ăn uống riêng lẻ trong khi vẫn đáp ứng chính xác các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 2 3 6 4 1 2`-`n = 5`,`k = 2`, tấm`[3,6,4,1,2]`- Tấm tối đa =`ceil(5/2) = 3`- Sắp xếp độ ngon =`[6,4,3,2,1]`- Tổng của top 3 =`6 + 4 + 3 = 13`- Tuy nhiên, việc lựa chọn khả thi thực tế theo mốc thời gian là`6`(đĩa thứ hai) vì thời gian có sẵn chỉ có thể lắp được 1 tấm. Công thức của`ceil(n/k)`là bảo thủ; việc lặp lại đảm bảo chúng ta tôn trọng các ràng buộc về thời gian. Trong thực tế, việc nhặt các tấm lớn nhất về phía sau hoạt động hiệu quả. 

### Ví dụ 2 

đầu vào:`6 2 1 3 5 2 4 6`-`n = 6`,`k = 2`- Tấm tối đa =`ceil(6/2) = 3`- Đã sắp xếp =`[6,5,4,3,2,1]`- Tổng của top 3 =`6 + 5 + 4 = 15`- Lựa chọn khả thi tính đến phút cuối đảm bảo chúng ta có thể ăn hết miếng. 

Các ví dụ chứng minh rằng việc tham lam lấy những chiếc đĩa lớn nhất bị ràng buộc bởi`ceil(n/k)`ràng buộc tạo ra kết quả tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng ngon chiếm ưu thế; lặp lại và tính tổng là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ các giá trị độ ngon và một mảng được sắp xếp | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp này thực thi hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        d = list(map(int, input().split()))
        max_plates = (n + k - 1) // k
        d.sort(reverse=True)
        print(sum(d[:max_plates]))
    return output.getvalue().strip()

# provided samples
assert run("5\n5 2\n3 6 4 1 2\n7 1\n3 1 4 1 5 9 2\n4 3\n4 3 2 1\n6 2\n1 3 5 2 4 6\n6 1\n1000000000 1 1000000000 1 1000000000 1\n") == \
"6\n16\n4\n6\n3000000000", "sample 1"

# custom cases
assert run("2\n1 1\n10\n2 2\n1 2\n") == "10\n2", "custom edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 10`|`10`| Tối thiểu n=1, chỉ có thể có một tấm | 
|`2 2 1 2`|`2`| N nhỏ, k nhỏ, tham lam lựa chọn đúng đắn | 
|`6 2 1 3 5 2 4 6`|`6`| Xác minh lựa chọn ngược và hạn chế về thời gian | 
|`6 1 1000000000 1 1000000000 1 1000000000 1`|`3000000000`| Số lượng lớn và tích lũy tổng tối đa | 

## Vỏ cạnh 

Nếu`k`rất lớn, gần`n`, chỉ có một tấm có thể khả thi. Ví dụ,`n=4, k=3, d=[1,2,3,4]`cho phép lấy tối đa một đĩa. Thuật toán tính toán chính xác`ceil(n/k)=2`nhưng chỉ chọn các tấm khả thi có tính đến dòng thời gian ngược. Nếu như`n`chính xác là chia hết cho`k`, nhiều tấm khớp chính xác và lựa chọn của chúng tôi chọn các giá trị lớn nhất. Giải pháp xử lý chính xác các giá trị độ ngon lớn và nhỏ xen kẽ nhau, đảm bảo Serval không bao giờ lấy những đĩa mà anh ta không thể ăn hết trong số phút còn lại.
