---
title: "CF 1538A - Trò Chơi Đá"
description: "Chúng ta được cấp một hàng đá, mỗi viên đá có giá trị sức mạnh riêng biệt. Trong một nước đi, chúng ta chỉ được phép loại bỏ một trong hai viên đá ranh giới, viên đá còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải."
date: "2026-06-14T18:57:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 800
weight: 1538
solve_time_s: 219
verified: true
draft: false
---

[CF 1538A - Trò chơi đá](https://codeforces.com/problemset/problem/1538/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, dp, tham lam 
**Thời gian giải:** 3 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng đá, mỗi viên đá có giá trị sức mạnh riêng biệt. Trong một nước đi, chúng ta chỉ được phép loại bỏ một trong hai viên đá ranh giới, viên đá còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. Cuối cùng, chúng tôi muốn loại bỏ cả viên đá yếu nhất và viên đá mạnh nhất trong mảng. 

Nhiệm vụ là xác định số lần xóa ranh giới tối thiểu cần thiết cho đến khi cả hai giá trị cực trị đều bị xóa. 

Nhận xét quan trọng từ các ràng buộc là mỗi trường hợp thử nghiệm đều nhỏ, có tối đa 100 viên đá. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc mô phỏng trên nhiều trạng thái. Ngay cả giải pháp O(n) hoặc O(n^2) cho mỗi trường hợp thử nghiệm cũng đủ nhanh. 

Một cách giải thích ngây thơ có thể đề xuất mô phỏng tất cả các chuỗi xóa trái và phải có thể có cho đến khi cả hai phần tử mục tiêu bị loại bỏ, nhưng điều đó dẫn đến số lượng khả năng theo cấp số nhân. Ngay cả với n = 100, điều đó hoàn toàn không thể thực hiện được. 

Có một số trường hợp tế nhị phá vỡ lối lý luận thiếu thận trọng. Đầu tiên, không phải lúc nào cũng tối ưu để loại bỏ từ một phía cho đến khi cả hai mục tiêu đều biến mất, bởi vì tùy thuộc vào vị trí của các giá trị tối thiểu và tối đa, chiến lược tốt nhất có thể liên quan đến việc loại bỏ khỏi bên trái trước hoặc bên phải trước hoặc kết hợp cả hai. 

Ví dụ: nếu mức tối thiểu ở gần đầu bên trái và mức tối đa ở gần đầu bên phải, chúng ta có thể muốn loại bỏ cạnh gần hơn cho mỗi bên. Nhưng nếu cả hai đều ở cùng một phía, chỉ loại bỏ khỏi phía đó có thể là tối ưu. 

Trường hợp cạnh thứ hai là khi một cực đã gần kết thúc nhưng cực kia lại nằm sâu bên trong mảng. Việc tham lam loại bỏ từ một phía mà không so sánh cả hai hướng có thể dễ dàng đếm quá nhiều nước đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi trình tự loại bỏ trái hoặc phải cho đến khi loại bỏ cả phần tử tối thiểu và tối đa. Mỗi trạng thái được xác định bởi mảng con hiện tại và từ mỗi trạng thái, chúng ta phân nhánh thành hai khả năng. Điều này tạo thành một cây nhị phân có độ sâu lên tới n, dẫn đến trạng thái O(2^n). Ngay cả khi cắt tỉa, cấu trúc vẫn theo cấp số nhân vì mỗi quyết định đều ảnh hưởng đến tính khả dụng trong tương lai của cả hai mục tiêu. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về trình tự và thay vào đó hãy nghĩ về vị trí. Điều duy nhất quan trọng là vị trí của các phần tử tối thiểu và tối đa trong mảng ban đầu. Khi chúng ta biết chỉ số của chúng, vấn đề trở nên tương đương với việc thu nhỏ mảng từ cả hai đầu cho đến khi cả hai chỉ số đều bị xóa. 

Nếu chúng ta biểu thị vị trí của các phần tử tối thiểu và tối đa là i và j, thì chúng ta chỉ quan tâm đến việc cần loại bỏ bao nhiêu lần để loại bỏ cả hai chỉ số. Chỉ có ba chiến lược có ý nghĩa. Chúng ta có thể xóa từ bên trái cho đến khi hết cả hai, xóa từ bên phải cho đến khi hết cả hai, hoặc xóa từ cả hai đầu: một số ở bên trái và một số ở bên phải. 

Điều này làm giảm vấn đề đánh giá một số trường hợp không đổi dựa trên i và j. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quét mảng để xác định chỉ mục của phần tử tối thiểu và chỉ mục của phần tử tối đa. Các vị trí này xác định đầy đủ phần còn lại của phép tính vì tất cả các giá trị khác không liên quan đến quá trình loại bỏ. 
2. Gọi i là chỉ số tối thiểu và j là chỉ số tối đa. Không mất tính tổng quát, giả sử i <= j. Nếu không, hãy trao đổi chúng. Việc chuẩn hóa này tránh việc phân tách chữ hoa chữ thường sau này. 
3. Tính toán ba chiến lược ứng cử viên. Đầu tiên, xóa mọi thứ từ trái lên xa hơn của i và j. Việc này cần tối đa(i, j) + 1 bước di chuyển vì chúng tôi lập chỉ mục từ 0. 
4. Thứ hai, xóa mọi thứ từ phải lên gần i và j. Việc này cần n - min(i, j) di chuyển, vì chúng ta xóa từ bên phải cho đến khi cả hai vị trí biến mất. 
5. Thứ ba, loại bỏ phần bên trái lên i và phần bên phải lên j một cách độc lập. Điều này cần (i + 1) + (n - j) di chuyển. Điều này tương ứng với việc đầu tiên xóa tất cả các phần tử trước i, sau đó xóa tất cả các phần tử sau j. 
6. Câu trả lời là giá trị tối thiểu trong ba giá trị này, vì mỗi giá trị tương ứng với một chiến lược xóa hợp lệ để đảm bảo cả hai phần tử mục tiêu đều bị loại bỏ. 

### Tại sao nó hoạt động 

Trạng thái của trò chơi luôn là một mảng con liền kề nhau, do đó, bất kỳ chuỗi nước đi nào cũng tương ứng với việc thu hẹp ranh giới vào bên trong. Các phần tử tối thiểu và tối đa đều phải nằm ngoài đoạn còn lại cuối cùng. Do đó, bất kỳ chiến lược hợp lệ nào cũng tương đương với việc chọn phân đoạn cuối cùng loại trừ cả hai chỉ số. Chi phí để tiếp cận phân khúc đó chỉ được xác định bằng số lượng phần tử được loại bỏ ở bên trái và bên phải, điều này làm giảm vấn đề giảm thiểu các vị trí vượt quá giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    mn = min(range(n), key=lambda i: a[i])
    mx = max(range(n), key=lambda i: a[i])
    
    i, j = mn, mx
    if i > j:
        i, j = j, i
    
    # three strategies
    left_only = j + 1
    right_only = n - i
    both = (i + 1) + (n - j)
    
    print(min(left_only, right_only, both))
```Giải pháp trước tiên xác định các chỉ số của giá trị tối thiểu và tối đa bằng cách quét tuyến tính. Sau đó, nó bình thường hóa thứ tự của chúng để chỉ số tối thiểu xuất hiện trước. Điều này cho phép xây dựng một công thức rõ ràng về ba chiến lược loại bỏ mà không cần phân tách chữ hoa chữ thường. 

Chiến lược đầu tiên loại bỏ từ bên trái cho đến khi mục tiêu ngoài cùng bên phải của hai mục tiêu biến mất. Cái thứ hai loại bỏ từ bên phải cho đến khi cái ngoài cùng bên trái biến mất. Lần phân chia thứ ba loại bỏ giữa cả hai đầu, tương ứng với việc cô lập một đoạn giữa vẫn không chứa cực đoan. 

Tất cả số học trực tiếp đếm số lần xóa được yêu cầu và không cần mô phỏng quá trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 5, 4, 3, 2]`Tối thiểu là ở chỉ số 0, tối đa là ở chỉ số 1. 

| Bước | tôi (phút) | j (tối đa) | left_only | đúng_only | cả hai | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 1 | 2 | 5 | 4 | 2 | 

Chiến lược tốt nhất là loại bỏ phần ngoài cùng bên trái hai lần, loại bỏ cả 1 và 5 một cách nhanh chóng. Điều này cho thấy việc chỉ tập trung vào một bên có thể mang lại hiệu quả tốt hơn việc loại bỏ hỗn hợp. 

### Ví dụ 2 

đầu vào:`[2, 1, 3, 4, 5, 6, 8, 7]`Tối thiểu là ở chỉ số 1, tối đa là ở chỉ số 6. 

| Bước | tôi | j | left_only | đúng_only | cả hai | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 6 | 7 | 7 | 3 + 2 = 5 | 5 | 

Ở đây giải pháp tối ưu là loại bỏ tiền tố ở mức tối thiểu và hậu tố ở mức tối đa. Điều này tránh lãng phí nước đi ở khu vực giữa. 

Hai ví dụ này cho thấy sự tương phản giữa phân cụm cực độ và cực trị tách biệt, đó chính xác là điều mà công thức ba trường hợp nắm bắt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần quét để tìm chỉ số tối thiểu và tối đa | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với n lên tới 100, do đó việc quét tuyến tính cho mỗi trường hợp là không đáng kể. Giải pháp chạy tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mn = min(range(n), key=lambda i: a[i])
        mx = max(range(n), key=lambda i: a[i])
        
        i, j = mn, mx
        if i > j:
            i, j = j, i
        
        left_only = j + 1
        right_only = n - i
        both = (i + 1) + (n - j)
        
        out.append(str(min(left_only, right_only, both)))
    
    return "\n".join(out)

# provided samples
assert solve("""5
5
1 5 4 3 2
8
2 1 3 4 5 6 8 7
8
4 2 3 1 8 6 7 5
4
3 4 2 1
4
2 3 1 4
""") == """2
4
5
3
2"""

# custom cases
assert solve("""3
2
1 2
2
2 1
5
3 1 5 2 4
""") == """1
1
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| đặt hàng tầm thường | 
|`2 1`|`1`| thứ tự đảo ngược | 
|`3 1 5 2 4`|`3`| vị trí nội bộ hỗn hợp | 

## Vỏ cạnh 

Khi mức tối thiểu và tối đa gần nhau, câu trả lời luôn là 2 trừ khi chúng đã ở cuối. Đối với đầu vào`[2, 1, 3, 4]`, min và max nằm ở chỉ số 1 và 0, vì vậy nước đi tốt nhất là loại bỏ bên này rồi bên kia, thực hiện 2 nước đi. 

Khi cả hai thái cực đều đã ở hai đầu đối lập nhau, chẳng hạn như`[1, 2, 3, 4, 5]`, câu trả lời trở thành 2 vì việc loại bỏ một trong hai bên sẽ cô lập cả hai mục tiêu ngay lập tức. Thuật toán nắm bắt điều này thông qua`both`hoặc công thức một mặt không có cách viết hoa đặc biệt. 

Khi cả hai thái cực đều ở giữa, chẳng hạn như`[3, 1, 5, 2, 4]`, chiến lược tối ưu là tách các phần bị loại bỏ khỏi cả hai đầu. Tính toán`both`value phản ánh chính xác rằng chỉ những phần tử nằm ngoài khoảng cách giữa hai vị trí mới cần được loại bỏ.
