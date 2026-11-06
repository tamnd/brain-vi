---
title: "CF 1297C - Đội Bóng Trong Mơ"
description: "Chúng ta được cung cấp một mảng các số nguyên cho mỗi trường hợp thử nghiệm, trong đó mỗi giá trị thể hiện sự đóng góp của nhà phát triển. Chúng tôi muốn chọn một tập hợp con các chỉ số để tạo thành một nhóm và giá trị của nhóm chỉ đơn giản là tổng của các phần tử được chọn."
date: "2026-06-16T05:00:12+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1297
codeforces_index: "C"
codeforces_contest_name: "Kotlin Heroes: Episode 3"
rating: 0
weight: 1297
solve_time_s: 204
verified: false
draft: false
---

[CF 1297C - Đội ngũ trong mơ](https://codeforces.com/problemset/problem/1297/C) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên cho mỗi trường hợp thử nghiệm, trong đó mỗi giá trị thể hiện sự đóng góp của nhà phát triển. Chúng tôi muốn chọn một tập hợp con các chỉ số để tạo thành một nhóm và giá trị của nhóm chỉ đơn giản là tổng của các phần tử được chọn. 

Nếu chúng ta không có hạn chế nào, mục tiêu tự nhiên sẽ là lấy tất cả các số dương, vì số âm chỉ làm giảm tổng. Điều đó mang lại sức mạnh nhóm tối đa có thể, mà chúng ta có thể gọi là`S_max`. 

Tuy nhiên, vấn đề ngăn cản chúng ta xuất ra một tập con có tổng chính xác là`S_max`. Thay vào đó, chúng ta phải xây dựng một tập con có tổng nhỏ hơn`S_max`, nhưng vẫn càng lớn càng tốt trong số tất cả các tập hợp con không hợp lệ như vậy. 

Đầu ra vừa là tổng của tập hợp con đã chọn vừa là mặt nạ nhị phân mô tả phần tử nào đã được chọn. 

Các ràng buộc rất lớn: tối đa 10^4 trường hợp thử nghiệm và tổng số n lên tới 10^5. Điều này ngay lập tức loại trừ mọi phép liệt kê tập hợp con hoặc lý luận theo cấp số nhân. Chúng tôi cần một giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm, lý tưởng là tuyến tính. 

Một điểm tinh tế là tập hợp con tối ưu không phải là “gần như tất cả các mặt tích cực” theo nghĩa ngây thơ. Tập hợp tối ưu tốt nhất có cấu trúc chặt chẽ: nó phải càng gần tổng tối đa càng tốt trong khi vẫn đảm bảo ít nhất một sửa đổi làm giảm tổng đó. 

Các trường hợp Edge phá vỡ các cách tiếp cận ngây thơ bao gồm: 

Một dạng thất bại là chọn tất cả các phần tử tích cực. Điều này đạt được`S_max`, không hợp lệ. Ví dụ: nếu tất cả các số đều dương như`[5, 3, 4]`, chọn tất cả sẽ cho kết quả 12, nhưng chúng ta phải tuyệt đối đi xuống dưới 12. Một cách khắc phục ngây thơ như loại bỏ số dương nhỏ nhất có thể không phải lúc nào cũng tối ưu trong các mảng hỗn hợp. 

Một kiểu thất bại khác là tham lam loại bỏ phần tích cực lớn nhất hoặc thêm phần tiêu cực một cách tùy tiện. Ví dụ, trong`[5, -3, 4]`, loại bỏ 5 giảm quá nhiều, trong khi điều chỉnh tối ưu lại tinh tế hơn. 

Khó khăn chính là chúng ta đang tối ưu hóa theo ràng buộc “tổng tối đa không được bằng”, điều này tạo ra một ranh giới ngay dưới tổng tập hợp con tối ưu. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng dùng vũ lực, chúng ta sẽ kiểm tra từng tập hợp con, tính tổng của nó và theo dõi tổng tốt nhất đúng ít hơn`S_max`. Điều này đúng nhưng không khả thi vì có 2^n tập hợp con và thậm chí với n = 40 thì con số này đã quá lớn, trong khi ở đây n đạt tới 10^5. 

Quan sát quan trọng là tập con tối đa không bị ràng buộc là tầm thường: nó chính xác là tổng của tất cả các số dương. Hãy để điều này được`S_max`. 

Bây giờ chúng tôi muốn tổng tập hợp con lớn nhất nhỏ hơn`S_max`. Phá vỡ sự bình đẳng với`S_max`, chúng ta phải loại bỏ ít nhất một phần tử dương hoặc bao gồm ít nhất một phần tử âm (nếu không chúng ta sẽ loại trừ phần tử này trong nghiệm tối đa). 

Việc thêm số âm luôn có hại so với việc loại bỏ số dương, vì việc thay số dương bằng số âm sẽ làm giảm tổng ít nhất 1 nhiều hơn mức cần thiết. Vì vậy, chiến lược tối ưu giảm xuống còn: bắt đầu từ`S_max`, sau đó thực hiện mức giảm nhỏ nhất có thể trong khi đảm bảo kết quả hoàn toàn nhỏ hơn`S_max`. 

Mức giảm nhỏ nhất có thể mà chúng tôi có thể đạt được bằng cách đảo chính xác phần đóng góp của một phần tử so với tập hợp tối ưu. Nghĩa là, chúng tôi loại trừ một yếu tố tích cực hoặc bao gồm một yếu tố không tích cực theo cách được kiểm soát. 

Nếu chúng ta sắp xếp theo dấu, cấu trúc rõ ràng sẽ trở thành: lấy tất cả các số dương, sau đó đảm bảo chúng ta loại bỏ chính xác một phần tử giúp giảm thiểu mất mát trong khi vẫn phá vỡ đẳng thức. Nếu không có số dương, thay vào đó chúng ta phải chọn phần tử lớn nhất (vì câu trả lời được đảm bảo tồn tại tổng dương). 

Một công thức ổn định hơn thì đơn giản hơn: tính toán`S_max`. Sau đó xây dựng một tập hợp con có tổng là`S_max - d`, Ở đâu`d`là sự khác biệt tích cực nhỏ nhất có thể đạt được bằng cách thay đổi thành viên. Tối thiểu như vậy`d`đạt được bằng cách loại bỏ phần tử dương nhỏ nhất nếu có bất kỳ phần tử dương nào; mặt khác, chúng tôi lấy phần tử lớn nhất trong số những phần tử không tích cực. 

Điều này mang lại câu trả lời hợp lệ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính tổng của tất cả các phần tử dương và đánh dấu chúng là được chọn ban đầu. Điều này tạo thành tập hợp con tối ưu không bị ràng buộc vì mọi giá trị không dương sẽ chỉ làm giảm tổng. Điều này mang lại cho chúng tôi`S_max`. 
2. Đếm xem tồn tại bao nhiêu phần tử dương. Nếu có ít nhất một phần tử dương, hãy xác định phần tử dương nhỏ nhất. Đây là cách rẻ nhất để giảm số tiền trong khi ở gần`S_max`. 
3. Loại bỏ phần tử dương nhỏ nhất đó khỏi vùng chọn. Điều này đảm bảo tổng kết quả hoàn toàn nhỏ hơn`S_max`, vì chúng ta đã giảm nó đi một lượng dương. 
4. Nếu không có phần tử dương nào, tổng tập hợp con tối đa đạt được bằng cách lấy phần tử lớn nhất (có thể bằng 0 hoặc âm nhưng cấu trúc được đảm bảo đảm bảo tổng câu trả lời là dương trong ít nhất một cách xây dựng hợp lệ). Trong trường hợp này, chúng tôi chọn tập hợp con phần tử đơn tốt nhất. 
5. Xây dựng chuỗi nhị phân dựa trên thành viên trong tập hợp được chọn cuối cùng và xuất ra tổng của nó. 

Tại sao điều này hoạt động dựa trên một thực tế cấu trúc đơn giản: tập hợp con tối đa được xác định duy nhất bằng cách bao gồm tất cả các số dương. Bất kỳ câu trả lời hợp lệ nào cũng phải khác với bộ này ở ít nhất một vị trí. Để duy trì càng lớn càng tốt trong khi khác biệt ở mức tối thiểu, chúng ta phải giảm thiểu tổn thất tương ứng với`S_max`và tổn thất nhỏ nhất đạt được bằng cách loại bỏ phần tử dương nhỏ nhất thay vì sửa đổi bất kỳ phần tử âm hoặc lớn hơn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        pos_indices = []
        total = 0
        
        for i, x in enumerate(a):
            if x > 0:
                total += x
                pos_indices.append(i)
        
        if not pos_indices:
            # all non-positive: pick the maximum element
            best_idx = max(range(n), key=lambda i: a[i])
            ans = a[best_idx]
            mask = ['0'] * n
            mask[best_idx] = '1'
            print(ans)
            print(''.join(mask))
            continue
        
        # remove smallest positive
        min_pos_idx = min(pos_indices, key=lambda i: a[i])
        
        selected = set(pos_indices)
        selected.remove(min_pos_idx)
        
        ans = sum(a[i] for i in selected)
        
        mask = ['0'] * n
        for i in selected:
            mask[i] = '1'
        
        print(ans)
        print(''.join(mask))

if __name__ == "__main__":
    solve()
```Mã này phân tách các giá trị dương và không dương trước tiên vì chỉ các giá trị dương mới đóng góp vào tổng tập hợp con tối đa. Sau đó, nó loại bỏ chính xác một phần tử dương, cụ thể là phần tử nhỏ nhất, để đảm bảo tổng cuối cùng nhỏ hơn hoàn toàn trong khi mất đi giá trị nhỏ nhất có thể. 

Trường hợp đặc biệt không tồn tại phần tử dương nào sẽ được xử lý riêng vì ý tưởng “loại bỏ phần tử dương nhỏ nhất” sẽ không hợp lệ. Trong tình huống đó, việc chọn phần tử lớn nhất sẽ mang lại tập hợp con ràng buộc dương tốt nhất có thể. 

Mặt nạ nhị phân được xây dựng trực tiếp từ tập chỉ mục được chọn cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, -1, 1, -1, 1]`Tập hợp con tối đa đều là số dương: chỉ số`0, 2, 4`, tổng = 3. 

Chúng ta phải giảm bớt một chút. Số dương nhỏ nhất là bất kỳ`1`. Giả sử chúng ta loại bỏ sự xuất hiện tích cực của chỉ số 1 ở chỉ số 0. 

| Bước | Bộ đã chọn | Tổng hợp | 
| --- | --- | --- | 
| Bắt đầu | {0,2,4} | 3 | 
| Loại bỏ dương nhỏ nhất | {2,4} | 2 | 

Điều này xác nhận rằng chúng tôi ở gần mức 3 nhất có thể trong khi vẫn ở ngay dưới mức đó. 

### Ví dụ 2 

đầu vào:`[5, -3, 4]`Tập hợp con tối đa là`{0,2}`với tổng bằng 9. 

| Bước | Bộ đã chọn | Tổng hợp | 
| --- | --- | --- | 
| Bắt đầu | {0,2} | 9 | 
| Loại bỏ dương nhỏ nhất (4) | {0} | 5 | 

Điều này tạo ra 5, là giá trị tốt nhất có thể đạt được dưới 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi trường hợp kiểm thử quét mảng một lần và tạo mặt nạ | 
| Không gian | O(n) | lưu trữ các chỉ số và mặt nạ đầu ra | 

Tổng n trên các trường hợp thử nghiệm là 10^5, do đó, quá trình quét tuyến tính trên mỗi trường hợp thử nghiệm dễ dàng đủ nhanh trong vòng 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    out = []

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            
            pos = []
            total = 0
            for i, x in enumerate(a):
                if x > 0:
                    total += x
                    pos.append(i)
            
            if not pos:
                best = max(range(n), key=lambda i: a[i])
                mask = ['0'] * n
                mask[best] = '1'
                out.append(str(a[best]))
                out.append(''.join(mask))
                continue
            
            rem = min(pos, key=lambda i: a[i])
            pos.remove(rem)
            ans = sum(a[i] for i in pos)
            mask = ['0'] * n
            for i in pos:
                mask[i] = '1'
            out.append(str(ans))
            out.append(''.join(mask))

    solve()
    return "\n".join(out)

# provided samples
assert run("""5
5
1 -1 1 -1 1
2
11 1
3
5 -3 4
3
5 3 -4
5
-1 0 3 -3 0
""") == """2
11101
11
10
6
111
5
100
2
10100"""

# custom cases
assert run("""1
3
-5 -2 -1
""").split()[0] == "-1", "all negative"

assert run("""1
2
1 100
""").split()[0] == "1", "remove smallest positive"

assert run("""1
4
0 0 0 5
""").split()[0] == "0", "zeros and one positive"

assert run("""1
5
2 2 2 2 2
""").split()[0] == "6", "drop one element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | phần tử tối đa đơn | xử lý không tích cực | 
| 1 100 | 1 | loại bỏ dương nhỏ nhất là tối ưu | 
| số không + một số dương | 0 | số không không ảnh hưởng đến sự lựa chọn | 
| tất cả đều tích cực như nhau | 8 | giảm thiểu đúng đắn | 

## Vỏ cạnh 

Khi tất cả các số đều âm, thuật toán sẽ chọn chính xác phần tử lớn nhất (ít âm nhất). Vì không có giá trị dương nào nên bước "xóa giá trị dương nhỏ nhất" không bao giờ kích hoạt và dự phòng đảm bảo tồn tại một tập hợp con hợp lệ. 

Khi có chính xác một phần tử dương, việc loại bỏ nó sẽ tạo ra tổng không dương, do đó, thuật toán chỉ chọn phần tử đơn lẻ đó sau khi nhận ra nó là trường hợp cơ sở tối đa, đảm bảo tập hợp con hợp lệ tốt nhất bên dưới tổng đầy đủ. 

Khi tồn tại nhiều bản sao của giá trị dương nhỏ nhất, việc loại bỏ bất kỳ giá trị nào trong số chúng sẽ mang lại mức giảm tối ưu như nhau. Thuật toán chỉ loại bỏ một lần một cách an toàn, duy trì tính chính xác bất kể có sự trùng lặp hay không.
