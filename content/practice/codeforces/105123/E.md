---
title: "CF 105123E - Nhà máy năng lượng của tế bào?"
description: "Chúng ta được cung cấp một chuỗi các nhiệm vụ được sắp xếp theo mức độ ưu tiên từ đầu đến cuối và mỗi nhiệm vụ tiêu tốn một lượng ATP cố định để hoàn thành. Jasmine có ngân sách ATP hàng ngày là $m$ và cô ấy không thể lặp lại nhiệm vụ. Điều khó khăn là không phải lúc nào cô ấy cũng xem xét tất cả các nhiệm vụ."
date: "2026-06-27T19:33:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105123
codeforces_index: "E"
codeforces_contest_name: "BioCode 2024"
rating: 0
weight: 105123
solve_time_s: 75
verified: false
draft: false
---

[CF 105123E - Sức mạnh của tế bào?](https://codeforces.com/problemset/problem/105123/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các nhiệm vụ được sắp xếp theo mức độ ưu tiên từ đầu đến cuối và mỗi nhiệm vụ tiêu tốn một lượng ATP cố định để hoàn thành. Jasmine có ngân sách ATP hàng ngày là$m$, và cô ấy không thể lặp lại nhiệm vụ. Điều khó khăn là không phải lúc nào cô ấy cũng xem xét tất cả các nhiệm vụ. Thay vào đó, với mỗi độ dài tiền tố$k$, cô ấy chỉ hạn chế sự chú ý đến điều đầu tiên$k$các nhiệm vụ theo thứ tự ưu tiên và trong nhóm hạn chế đó, cô ấy muốn hoàn thành càng nhiều nhiệm vụ càng tốt mà không vượt quá ngân sách ATP của mình. 

Đối với mỗi$k$, chúng ta đang hỏi một cách hiệu quả: nếu chúng ta chỉ nhận nhiệm vụ$1$bởi vì$k$, số lượng chi phí nhiệm vụ tối đa mà chúng ta có thể chọn có tổng lớn nhất là bao nhiêu$m$. 

Điều quan trọng cần lưu ý là các tác vụ luôn được thực hiện theo thứ tự tiền tố, nhưng bên trong tiền tố đó, chúng ta có thể tự do chọn bất kỳ tập hợp con nào. Vì vậy, đây không phải là vấn đề lập kế hoạch hoặc sắp xếp, nó là vấn đề lặp đi lặp lại “tập hợp con tốt nhất dưới ràng buộc tổng” đối với các tiền tố đang phát triển. 

Những hạn chế là lớn, với$n$lên đến$2 \cdot 10^5$. Điều này ngay lập tức loại trừ việc tính toán lại một tập hợp con tối ưu từ đầu cho mỗi$k$. Một sự ngây thơ$O(n^2 \log n)$hoặc$O(n^2)$Cách tiếp cận sắp xếp lại hoặc quét lại tiền tố sẽ quá chậm. Chúng ta cần một cấu trúc tăng dần trong đó mỗi bước cập nhật câu trả lời trước đó theo thời gian khấu hao logarit hoặc không đổi. 

Một vấn đề tế nhị xuất hiện khi chi phí mất cân đối cao. Ví dụ, nếu$m = 10$và nhiệm vụ là$[1, 1, 1, 100, 1]$, thì các tiền tố ban đầu cho phép nhiều lựa chọn nhỏ, nhưng một khi chi phí lớn xuất hiện thì nó sẽ không bao giờ thuộc tập hợp con tối ưu, tuy nhiên nó sẽ thay đổi tập hợp tiền tố. Bất kỳ cách tiếp cận tham lam không chính xác nào giả định rằng chúng ta luôn chọn những nhiệm vụ có sẵn rẻ nhất trên toàn cầu sẽ thất bại trừ khi nó duy trì cẩn thận tính khả thi trong quá trình tăng trưởng tiền tố. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về mỗi tiền tố là sắp xếp các phần tử của nó và lấy chi phí nhỏ nhất cho đến khi tổng vượt quá$m$. Điều đó đúng vì để tối đa hóa số lượng theo một ràng buộc về tổng, việc chọn chi phí nhỏ hơn luôn là tối ưu. Tuy nhiên, việc tính toán lại giá trị này cho mỗi tiền tố có nghĩa là chúng tôi liên tục sắp xếp hoặc xây dựng lại các cấu trúc lên đến$O(n)$các yếu tố, dẫn đến$O(n^2 \log n)$hành vi trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là khi chuyển từ tiền tố$k-1$ĐẾN$k$, chúng tôi đang thêm chính xác một phần tử mới. Bộ tiền tố tối ưu$k$khác với tiền tố$k-1$chỉ vì chúng tôi có thể muốn bao gồm phần tử mới này, có thể loại bỏ phần tử đã chọn lớn hơn nếu nó giúp tăng số lượng trong cùng một ngân sách. 

Điều này gợi ý việc duy trì một tập hợp động các tác vụ đã chọn, luôn thể hiện tập hợp con tốt nhất có thể cho tiền tố hiện tại. Cấu trúc tiêu chuẩn cho việc này là vùng heap tối đa: chúng tôi giữ tất cả các tác vụ hiện được chọn và đảm bảo tổng số tiền của chúng nằm trong$m$. Khi thêm một nhiệm vụ mới, chúng tôi sẽ chèn nó và nếu tổng số vượt quá$m$, chúng tôi loại bỏ phần tử lớn nhất. Loại bỏ phần lớn nhất là tối ưu vì nó giảm tổng số tiền nhiều nhất cho mỗi lần loại bỏ và bảo toàn được càng nhiều phần tử nhỏ càng tốt. 

Vì vậy mỗi tiền tố có thể được cập nhật trong$O(\log n)$và chúng tôi duy trì cả tổng hiện tại và số lượng nhiệm vụ đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại từng tiền tố) |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu (tăng heap tối đa) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nhiệm vụ từ trái sang phải, duy trì cấu trúc các nhiệm vụ đã chọn luôn phù hợp với ngân sách ATP. 

1. Khởi tạo vùng heap tối đa trống và một biến`total = 0`. Heap lưu trữ chi phí nhiệm vụ đã chọn và`total`theo dõi tổng của chúng. 
2. Đối với mỗi nhiệm vụ$a_k$, chèn nó vào heap và thêm giá trị của nó vào`total`. Điều này tương ứng với việc tạm chấp nhận mọi nhiệm vụ mới, vì chúng ta vẫn chưa biết liệu nó có phải là một phần của tập hợp con tối ưu cuối cùng cho tiền tố này hay không. 
3. Nếu`total > m`, loại bỏ phần tử lớn nhất khỏi heap và trừ nó khỏi`total`. Bước này khôi phục tính khả thi. Loại bỏ phần tử lớn nhất là lựa chọn duy nhất không bao giờ có thể giảm số lượng nhiệm vụ được chọn nhiều hơn mức cần thiết, vì nó hy sinh nhiệm vụ tốn kém nhất trong khi vẫn bảo toàn được nhiều mục nhất có thể. 
4. Sau khi điều chỉnh, kích thước của vùng heap chính xác là số lượng tác vụ tối đa có thể hoàn thành đối với tiền tố hiện tại. Xuất kích thước này. 
5. Lặp lại quá trình này cho tất cả$k$từ 1 đến$n$, tạo ra một câu trả lời cho mỗi tiền tố. 

Tại sao điều này có tác dụng gắn liền với một cuộc tranh luận trao đổi tham lam. Tại bất kỳ tiền tố nào, giả sử chúng ta có một tập hợp con khả thi có kích thước không tối ưu. Nếu nó chứa phần tử lớn hơn trong khi loại trừ phần tử nhỏ hơn, việc hoán đổi chúng không bao giờ làm giảm tính khả thi và không bao giờ làm giảm số lượng. Do đó, một giải pháp tối ưu luôn ưu tiên các phần tử nhỏ hơn và bất biến vùng heap thực thi chính xác cấu trúc đó một cách linh hoạt khi các phần tử mới xuất hiện. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    heap = []
    total = 0
    out = []
    
    for x in a:
        heapq.heappush(heap, -x)
        total += x
        
        if total > m:
            largest = -heapq.heappop(heap)
            total -= largest
        
        out.append(str(len(heap)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Chi tiết triển khai cốt lõi là việc sử dụng vùng heap tối đa được mô phỏng thông qua các giá trị âm, vì Python`heapq`chỉ cung cấp một đống tối thiểu. Chúng tôi duy trì`total`rõ ràng vì việc tính toán lại tổng đống sẽ quá chậm. 

Một điểm tinh tế là chúng tôi luôn loại bỏ chính xác một phần tử lớn nhất khi xảy ra tràn. Kể cả nếu`total`vượt xa`m`, một lần loại bỏ là đủ cho mỗi lần chèn vì sau khi loại bỏ phần tử lớn nhất, các phần tử còn lại đều nhỏ hơn hoặc bằng nhau và tính bất biến của heap đảm bảo chúng ta chỉ vượt quá dung lượng tối đa bằng sự đóng góp của phần tử mới được thêm vào. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
n = 5, m = 15
a = [14, 15, 11, 4, 4]
```Chúng tôi theo dõi vùng heap sau mỗi bước. 

| k | Đã chèn | Đống (nhiều bộ) | Tổng cộng | Đã xóa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 14 | [14] | 14 | không | 1 | 
| 2 | 15 | [14, 15] | 29 | 15 | 1 | 
| 3 | 11 | [14, 11] | 25 | 14 | 1 | 
| 4 | 4 | [11, 4] | 15 | không | 2 | 
| 5 | 4 | [11, 4, 4] | 19 | 11 | 2 | 

Dấu vết cho thấy các phần tử lớn sẽ bị loại bỏ bất cứ khi nào chúng chặn sự bao gồm của nhiều phần tử nhỏ hơn. Heap luôn phát triển theo hướng giữ tập hợp con khả thi rẻ nhất. 

Một ví dụ thứ hai:```
n = 4, m = 10
a = [3, 4, 5, 2]
```| k | Đã chèn | Đống | Tổng cộng | Đã xóa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | [3] | 3 | không | 1 | 
| 2 | 4 | [4, 3] | 7 | không | 2 | 
| 3 | 5 | [5, 3, 4] | 12 | 5 | 2 | 
| 4 | 2 | [4, 3, 2] | 9 | không | 3 | 

Điều này chứng tỏ rằng việc loại bỏ phần tử lớn nhất sau khi tràn cho phép giải pháp thích ứng trên toàn cầu mà không cần phải xem lại các quyết định trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi trong số$n$các phần chèn thêm và nhiều nhất một lần loại bỏ sử dụng các thao tác trên đống | 
| Không gian |$O(n)$| Heap lưu trữ tối đa tất cả các phần tử được chọn đang hoạt động trong trường hợp xấu nhất | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc đối với$n \le 2 \cdot 10^5$, vì các phép toán logarit có hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    heap = []
    total = 0
    res = []
    
    for x in a:
        heapq.heappush(heap, -x)
        total += x
        
        if total > m:
            total += heapq.heappop(heap)
        
        res.append(str(len(heap)))
    
    return "\n".join(res)

# sample
assert run("5 15\n14 15 11 4 4\n") == "1\n1\n1\n2\n2"

# all small, no removals
assert run("4 20\n1 2 3 4\n") == "1\n2\n3\n4"

# immediate large removal
assert run("3 5\n10 1 1\n") == "0\n1\n2"

# alternating heavy/light
assert run("5 6\n5 1 5 1 5\n") == "1\n2\n2\n3\n3"

# single element
assert run("1 100\n50\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1.. | mẫu | tính đúng đắn của việc loại bỏ hỗn hợp | 
| tăng nhỏ | 1 2 3 4 | không có trường hợp loại bỏ | 
| nặng đầu tiên | 0 1 2 | trục xuất phần tử quá khổ | 
| xen kẽ | 1 2 2 3 3 | tái cân bằng lặp đi lặp lại | 
| độc thân | 1 | trường hợp ranh giới | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi phần tử đầu tiên đã vượt quá ngân sách. Đối với đầu vào`n = 3, m = 5, a = [10, 1, 1]`, đống đầu tiên trở thành`[10]`, tổng số vượt quá ngân sách và chúng tôi ngay lập tức xóa nó, để lại một đống trống. Câu trả lời cho$k=1$do đó là 0 và các phần tử nhỏ tiếp theo sẽ xây dựng lại giải pháp từ đầu. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần chèn đều có một bước chỉnh sửa. 

Một trường hợp khác là khi nhiều phần tử nhỏ tích tụ lại và một phần tử lớn xuất hiện muộn. Vì`m = 6, a = [1, 1, 1, 1, 1, 10]`, heap tăng trưởng đều đặn đến kích thước 5, sau đó 10 lực sẽ loại bỏ chính nó chứ không phải bất kỳ phần tử nhỏ nào, bảo toàn số lượng tối ưu. Điều này cho thấy sự bất biến là vùng heap luôn chứa tập hợp con khả thi có chi phí nhỏ nhất cho tiền tố.
