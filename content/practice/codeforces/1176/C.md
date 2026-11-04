---
title: "CF 1176C - Thất bại!"
description: "Chúng ta được cung cấp một chuỗi dài trong đó mọi phần tử được đảm bảo là một trong sáu giá trị cố định: 4, 8, 15, 16, 23, 42. Nhiệm vụ không phải là sắp xếp lại hoặc sửa đổi các giá trị mà chỉ xóa các phần tử để phần còn lại có thể được phân chia thành các chuỗi có thứ tự hoàn chỉnh có độ dài sáu."
date: "2026-06-13T10:08:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1176
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 565 (Div. 3)"
rating: 1300
weight: 1176
solve_time_s: 247
verified: true
draft: false
---

[CF 1176C - Thất bại!](https://codeforces.com/problemset/problem/1176/C) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 4 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài trong đó mọi phần tử được đảm bảo là một trong sáu giá trị cố định: 4, 8, 15, 16, 23, 42. Nhiệm vụ không phải là sắp xếp lại hoặc sửa đổi các giá trị mà chỉ xóa các phần tử để phần còn lại có thể được phân chia thành các chuỗi có thứ tự hoàn chỉnh có độ dài sáu. Mỗi chuỗi hợp lệ phải tuân theo thứ tự nghiêm ngặt 4 → 8 → 15 → 16 → 23 → 42 và mỗi phần tử trong mảng còn lại phải thuộc chính xác một vị trí chuỗi như vậy. 

Mục tiêu là tối đa hóa số lượng chuỗi đầy đủ mà chúng ta có thể hình thành từ thứ tự ban đầu, bởi vì mỗi chuỗi đầy đủ gồm sáu chuỗi đều đóng góp vào các phần tử được giữ lại và mọi thứ bên ngoài các chuỗi này phải bị xóa. Khi chúng ta biết số lượng chuỗi đầy đủ tối đa, câu trả lời chỉ đơn giản là tổng số phần tử trừ đi sáu lần con số đó. 

Các ràng buộc rất lớn, lên tới 500.000 phần tử. Điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$O(n \log n)$cách tiếp cận với trạng thái nặng cho mỗi phần tử. Giải pháp phải tuyến tính hoặc rất gần với tuyến tính, xử lý mảng trong một lần truyền hoặc một số lần truyền không đổi nhỏ. 

Một trường hợp thất bại tinh tế xuất hiện khi chiến lược tham lam chỉ theo dõi tần số của từng số một cách độc lập. Ví dụ: nếu chúng ta nhìn thấy nhiều số 8 trước bất kỳ số 4 nào hoặc nhiều số 23 trước số 16, thì cách tính ngây thơ sẽ đánh giá quá cao các chuỗi có thể có. Sự phụ thuộc thứ tự rất nghiêm ngặt, vì vậy chúng ta phải tôn trọng trình tự, không chỉ tổng số. Một trường hợp phức tạp khác là xen kẽ nhiều chuỗi từng phần; cùng một giá trị có thể được sử dụng trong các chuỗi khác nhau tùy thuộc vào cách các phần tử trước đó được tiêu thụ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng việc xây dựng mọi chuỗi có thể. Chúng ta có thể cố gắng gán từng phần tử cho một chuỗi một cách tham lam, duy trì tất cả các chuỗi được xây dựng một phần. Mỗi số mới sẽ cố gắng mở rộng bất kỳ chuỗi phần tương thích nào hoặc bắt đầu một chuỗi mới nếu đó là số 4. Điều này nhanh chóng trở nên phức tạp vì chúng ta phải theo dõi nhiều chuỗi có thể chưa hoàn chỉnh. Trong trường hợp xấu nhất, khi các phần tử xen kẽ nhau theo mô hình đối nghịch, chúng ta có thể kết thúc bằng$O(n)$chuỗi từng phần đang hoạt động và mỗi bản cập nhật có thể quét nhiều chuỗi trong số đó, dẫn đến$O(n^2)$hành vi. 

Quan sát quan trọng là tất cả các chuỗi đều giống hệt nhau và có trật tự nghiêm ngặt. Chúng tôi không cần theo dõi từng chuỗi riêng lẻ, chỉ cần theo dõi có bao nhiêu chuỗi hiện đang chờ mỗi giai đoạn hoàn thành. Thay vì theo dõi trình tự, chúng tôi theo dõi số lượng chuỗi từng phần đã đạt đến từng tiền tố: bao nhiêu đã thấy 4, bao nhiêu đã thấy 4→8, bao nhiêu chuỗi đã thấy 4→8→15, v.v. 

Điều này biến vấn đề thành một đường dẫn. Mỗi số hoặc nâng cao một chuỗi từ giai đoạn$i$ĐẾN$i+1$hoặc bị bỏ qua nếu không có chuỗi nào ở giai đoạn đó. Quy tắc tham lam trở thành: luôn kéo dài giai đoạn sớm nhất có thể. 

Điều này làm giảm vấn đề thành mô phỏng giống DP 6 giai đoạn cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (theo dõi chuỗi một cách rõ ràng) | O(n²) | O(n) | Quá chậm | 
| Mô phỏng tham lam đếm từng giai đoạn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi ánh xạ từng giá trị vào vị trí của nó trong chuỗi: 4 là giai đoạn 0, 8 là giai đoạn 1, 15 là giai đoạn 2, 16 là giai đoạn 3, 23 là giai đoạn 4, 42 là giai đoạn 5. 

Chúng tôi duy trì một mảng`cnt[6]`, Ở đâu`cnt[i]`là số lượng các chuỗi một phần hiện đang chờ giá trị ở giai đoạn`i`. 

Chúng tôi cũng duy trì số lượng chuỗi hoàn chỉnh mà chúng tôi đã hoàn thành. 

1. Khởi tạo tất cả số đếm về 0. Chúng ta sẽ xử lý các số từ trái sang phải, giữ nguyên các ràng buộc về thứ tự một cách tự nhiên. 
2. Khi chúng ta đọc một giá trị, hãy xác định chỉ số giai đoạn của nó. 
3. Nếu giá trị là 4 (giai đoạn 0), chúng tôi luôn bắt đầu một chuỗi một phần mới bằng cách tăng`cnt[0]`. Đây là giá trị duy nhất có thể bắt đầu một chuỗi. 
4. Đối với bất kỳ giá trị nào khác ở giai đoạn`i > 0`, chúng tôi cố gắng mở rộng chuỗi hiện đang chờ giá trị này. Nếu như`cnt[i-1] > 0`, chúng tôi giảm`cnt[i-1]`và tăng dần`cnt[i]`. Điều này thể hiện việc sử dụng một phần chuỗi đã bắt đầu trước đó và thúc đẩy nó. 
5. Nếu giá trị là 42 (giai đoạn 5), thì sau khi mở rộng chuỗi sang giai đoạn 5, chúng tôi sẽ hoàn thành nó ngay lập tức. Vì vậy sau khi giảm`cnt[4]`, chúng tôi tăng số lượng chuỗi đã hoàn thành thay vì lưu trữ chuỗi giai đoạn 5. 
6. Nếu không có chuỗi nào để mở rộng, chúng tôi sẽ ngầm loại bỏ phần tử đó bằng cách không làm gì cả. Điều này tương ứng với việc nó là một trong những phần tử bị loại bỏ trong giải pháp tối ưu. 

Ý tưởng quan trọng là chúng tôi không bao giờ trì hoãn nhiệm vụ. Mỗi phần tử được sử dụng ngay lập tức nếu có thể và luôn ở giai đoạn tương thích sớm nhất. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cấu trúc của một phần chuỗi đều được nắm bắt đầy đủ bằng số lượng chuỗi đang chờ mỗi giá trị được yêu cầu tiếp theo. Vì tất cả các chuỗi đều có cấu trúc giống hệt nhau và tất cả các thành phần đều được xử lý theo thứ tự, nên bất kỳ quyết định nào gán giá trị cho giai đoạn sau trong khi giai đoạn trước đó vẫn có sẵn sẽ chỉ làm giảm tính linh hoạt trong tương lai. Việc tham lam mở rộng giai đoạn sớm nhất có thể sẽ bảo toàn được khả năng tối đa để hoàn thành các chuỗi đầy đủ sau này. Điều này duy trì một bất biến rằng`cnt[i]`luôn là số lượng tối đa các chuỗi từng phần có thể đạt tới giai đoạn`i`sử dụng tiền tố được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    order = {4: 0, 8: 1, 15: 2, 16: 3, 23: 4, 42: 5}
    cnt = [0] * 6
    completed = 0
    
    for x in a:
        i = order[x]
        
        if i == 0:
            cnt[0] += 1
        else:
            if cnt[i - 1] > 0:
                cnt[i - 1] -= 1
                if i == 5:
                    completed += 1
                else:
                    cnt[i] += 1
    
    print(n - completed * 6)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng phần tử chính xác một lần. Từ điển ánh xạ từng giá trị vào chỉ mục giai đoạn của nó để quá trình chuyển đổi diễn ra theo thời gian không đổi. các`cnt`mảng theo dõi có bao nhiêu chuỗi một phần đang chờ ở mỗi giai đoạn. Khi đạt đến 42, chúng tôi chỉ tính số lần hoàn thành và không lưu trữ thêm vì các chuỗi đã hoàn thành không còn tham gia vào quá trình chuyển đổi trong tương lai. 

Một cạm bẫy triển khai phổ biến là cho phép 42 tạo trạng thái mới thay vì chấm dứt chuỗi một cách không chính xác. Một trường hợp khác là quên đảm bảo rằng mỗi phần tử tiến lên chính xác một chuỗi nhiều nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 8 15 16 23
```| Bước | Giá trị | cnt[0] | cnt[1] | cnt[2] | cnt[3] | cnt[4] | Đã hoàn thành | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 8 | 0 | 1 | 0 | 0 | 0 | 0 | 
| 3 | 15 | 0 | 0 | 1 | 0 | 0 | 0 | 
| 4 | 16 | 0 | 0 | 0 | 1 | 0 | 0 | 
| 5 | 23 | 0 | 0 | 0 | 0 | 1 | 0 | 

Không có chuỗi nào đạt tới 42 nên số hoàn thành vẫn là 0. Câu trả lời là$5 - 0 = 5$. 

Điều này chứng tỏ rằng tiến độ một phần mà không hoàn thành đầy đủ sẽ không đóng góp gì vào điểm số cuối cùng. 

### Ví dụ 2 

đầu vào:```
12
4 4 8 15 8 16 15 23 16 42 23 42
```| Bước | Giá trị | cnt[0] | cnt[1] | cnt[2] | cnt[3] | cnt[4] | Đã hoàn thành | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 4 | 2 | 0 | 0 | 0 | 0 | 0 | 
| 3 | 8 | 1 | 1 | 0 | 0 | 0 | 0 | 
| 4 | 15 | 1 | 0 | 1 | 0 | 0 | 0 | 
| 5 | 8 | 0 | 1 | 1 | 0 | 0 | 0 | 
| 6 | 16 | 0 | 1 | 0 | 1 | 0 | 0 | 
| 7 | 15 | 0 | 0 | 1 | 1 | 0 | 0 | 
| 8 | 23 | 0 | 0 | 1 | 0 | 1 | 0 | 
| 9 | 16 | 0 | 0 | 0 | 1 | 1 | 0 | 
| 10 | 42 | 0 | 0 | 0 | 1 | 0 | 1 | 
| 11 | 23 | 0 | 0 | 0 | 0 | 1 | 1 | 
| 12 | 42 | 0 | 0 | 0 | 0 | 0 | 2 | 

Dấu vết này cho thấy cách quản lý đồng thời nhiều chuỗi một phần chồng chéo và cách chỉ những lần hoàn thành được sắp xếp hợp lý mới đóng góp vào kết quả cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử cập nhật tối đa một lần chuyển đổi trong thời gian không đổi | 
| Không gian | O(1) | Chỉ có sáu bộ đếm được duy trì bất kể kích thước đầu vào | 

Thuật toán tuyến tính theo kích thước mảng, phù hợp thoải mái trong giới hạn 500.000 phần tử. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào quy mô đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    order = {4: 0, 8: 1, 15: 2, 16: 3, 23: 4, 42: 5}
    
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    cnt = [0] * 6
    completed = 0
    
    for x in a:
        i = order[x]
        if i == 0:
            cnt[0] += 1
        else:
            if cnt[i - 1] > 0:
                cnt[i - 1] -= 1
                if i == 5:
                    completed += 1
                else:
                    cnt[i] += 1
    
    return str(n - completed * 6)

# provided sample
assert run("5\n4 8 15 16 23\n") == "5"

# custom cases
assert run("6\n4 8 15 16 23 42\n") == "0"
assert run("12\n4 8 15 16 23 42 4 8 15 16 23 42\n") == "0"
assert run("6\n4 8 15 16 42 23\n") == "6"
assert run("1\n4\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn không hoàn chỉnh | 1 | xử lý đầu vào tối thiểu | 
| hai chuỗi hoàn hảo | 0 | hoàn thành nhiều lần | 
| sai thứ tự bên trong chuỗi | 6 | vi phạm ràng buộc đặt hàng | 
| phần tử khởi đầu đơn | 1 | hành vi giai đoạn cơ sở | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các phần tử đến đúng thứ tự một phần nhưng bị “hỏng” trước khi hoàn thành. Đối với đầu vào như`4 8 15 16 42 23`, thuật toán sẽ tiến tới giai đoạn 3 thành công, không hoàn thành chuỗi ở 42 và sau đó coi 23 là vô dụng. Số lượng hoàn thành cuối cùng vẫn bằng 0, phù hợp với thực tế là không tồn tại chuỗi đầy đủ. 

Một trường hợp cạnh khác là có quá nhiều phần tử ở giữa ở đầu chẳng hạn như nhiều số 8 trước bất kỳ số 4 nào. Thuật toán chỉ đơn giản là loại bỏ các số 8 chưa khớp vì`cnt[0]`là số không. Điều này ngăn cản việc xây dựng chuỗi không hợp lệ và đảm bảo không có sự tiến bộ sớm. 

Trường hợp cạnh cuối cùng là sự đan xen nặng nề của nhiều chuỗi. Thuật toán phân phối các giai đoạn có sẵn một cách tự nhiên trên nhiều chuỗi một phần vì mỗi bộ đếm giai đoạn thể hiện năng lực chuỗi độc lập. Điều này tránh được sự cần thiết phải theo dõi chuỗi rõ ràng trong khi vẫn duy trì sự kết hợp tối ưu.
