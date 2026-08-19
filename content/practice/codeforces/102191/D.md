---
title: "CF 102191D - Ngày Hình Ảnh"
description: "Chúng ta có số học sinh chẵn, chia thành các cặp bạn cố định. Mỗi cặp phải chiếm hai vị trí liên tiếp nhưng chúng ta có thể chọn bạn nào đến trước."
date: "2026-08-18T19:39:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "D"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 325
verified: false
draft: false
---

[CF 102191D - Ngày hội hình ảnh](https://codeforces.com/problemset/problem/102191/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có số học sinh chẵn, chia thành các cặp bạn cố định. Mỗi cặp phải chiếm hai vị trí liên tiếp nhưng chúng ta có thể chọn bạn nào đến trước. Nhiệm vụ là sắp xếp thứ tự các cặp và định hướng từng cặp sao cho mảng chiều cao hoàn chỉnh trước tiên không bao giờ giảm và sau một số đỉnh, không bao giờ tăng. Đầu ra có thể là bất kỳ sự sắp xếp hợp lệ nào, hoặc`-1`nếu không có sự sắp xếp như vậy tồn tại. Điều này phù hợp với cấu trúc của tuyên bố vấn đề chính thức. 

Với mỗi cặp, hãy quên thứ tự ban đầu của nó và viết nó thành một khoảng`[l, r]`, Ở đâu`l`là chiều cao ngắn hơn và`r`là chiều cao cao hơn. Nếu cặp xuất hiện ở phía tăng dần của hình ảnh thì nó phải được viết là`l, r`. Nếu nó xuất hiện ở phía giảm thì phải viết là`r, l`. 

Xét hai cặp đặt liên tiếp ở phía tăng dần. Nếu khoảng của chúng là`[l1, r1]`Và`[l2, r2]`, bốn chiều cao của họ trở thành`l1, r1, l2, r2`. Để giá trị này không giảm, chúng ta cần`r1 <= l2`. Nói cách khác, hai khoảng không thể trùng nhau, mặc dù được phép chạm vào điểm cuối. Lập luận tương tự áp dụng cho các cặp ở phía giảm dần, ngoại trừ thứ tự của chúng bị đảo ngược. 

Điều này mang lại sự cải cách trung tâm. Chúng ta phải chia tất cả các khoảng cặp thành hai nhóm, trong đó các khoảng trong cùng một nhóm không chồng chéo lên nhau. Một nhóm sẽ tạo thành nửa tăng và nhóm còn lại sẽ tạo thành nửa giảm. 

Sự ràng buộc`n <= 3 * 10^5`nghĩa là có thể có tới`150000`cặp. Một thuật toán bậc hai sẽ thực hiện gần đúng`2.25 * 10^10`so sánh cặp trong trường hợp xấu nhất, vượt xa giới hạn hai giây có thể xử lý. Chúng tôi cần một`O(n log n)`giải pháp, trong đó hệ số logarit xuất phát từ việc sắp xếp. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai sử dụng các bất đẳng thức nghiêm ngặt. Đầu tiên, khoảng cách chạm là tương thích. Ví dụ,```
4
1 2
2 3
```có sự sắp xếp hợp lệ`1 2 2 3`. Hai khoảng`[1,2]`Và`[2,3]`chạm ở độ cao`2`và chiều cao liền kề bằng nhau được cho phép. Một triển khai sử dụng`l > previous_r`thay vì`l >= previous_r`sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp thứ hai là khi đỉnh nằm bên trong một cặp. Ví dụ,```
4
1 4
2 3
```có thể được sắp xếp như`2 3 4 1`. Cặp đầu tiên nằm ở phía tăng dần, trong khi cặp thứ hai là cặp chứa đỉnh và được viết theo thứ tự giảm dần. Các khoảng`[1,4]`Và`[2,3]`chồng chéo lên nhau, do đó, coi toàn bộ vấn đề là yêu cầu mọi khoảng thời gian phải rời rạc sẽ bác bỏ nó một cách không chính xác. 

Trường hợp thứ ba là ba khoảng chồng lên nhau ở một độ cao chung:```
6
1 5
2 4
3 6
```Điều này không có giải pháp. Ở độ cao`3`, cả ba khoảng thời gian cặp đều hoạt động. Vì chỉ có hai mặt của ngọn núi nên hai trong số các cặp này sẽ phải thuộc cùng một phía, nhưng các khoảng chồng lấp không thể cùng tồn tại trên một mặt đơn điệu. 

Cuối cùng, chiều cao bằng nhau là hoàn toàn hợp lệ. Ví dụ,```
4
3 3
3 3
```chỉ đơn giản là có thể sản xuất`3 3 3 3`. Một giải pháp phải xử lý một khoảng`[x,x]`giống như mọi khoảng thời gian khác và phải cho phép nhiều khoảng thời gian như vậy được gán cho các phía khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là coi mỗi cặp như một khối không thể chia được, thử mọi thứ tự của các khối, thử cả hai hướng cho mỗi khối và kiểm tra xem mảng chiều cao thu được có phải là một ngọn núi hay không. Với`m = n/2`cặp, có`m!`cách sắp xếp các cặp và`2^m`cách để định hướng chúng. Mỗi ứng viên đều yêu cầu`O(n)`làm việc để kiểm tra, vì vậy tổng công việc là`O(n * m! * 2^m)`. Đối với đầu vào tối đa, đây là`3 * 10^5 * 150000! * 2^150000`, điều này không khả thi chút nào. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng mọi vị trí và hướng có thể có. Vấn đề là hầu hết những lựa chọn đó đều không cần thiết. Việc mỗi cặp có đúng hai phần tử mang lại cho chúng ta một cấu trúc mạnh mẽ hơn nhiều. 

Sắp xếp hai chiều cao trong mỗi cặp và xem cặp đó dưới dạng khoảng`[l,r]`. Về phía tăng dần, cặp này phải xuất hiện dưới dạng`l,r`. Hai cặp tăng liên tiếp có giá trị chính xác khi khoảng đầu tiên kết thúc trước hoặc ở đầu khoảng thứ hai. Như vậy phía tăng là một chuỗi các khoảng không chồng lên nhau. Bên giảm có tính chất tương tự sau khi đảo ngược thứ tự. 

Do đó, chúng tôi đã giảm vấn đề thành việc phân chia các khoảng thành hai chuỗi các khoảng không chồng chéo. Đây là quan sát quan trọng vì việc lập kế hoạch theo khoảng thời gian có cấu trúc tham lam đơn giản. 

Sắp xếp tất cả các khoảng theo điểm cuối bên trái của chúng. Duy trì điểm cuối ngoài cùng bên phải hiện đang được sử dụng trong mỗi chuỗi trong số hai chuỗi. Đối với một khoảng thời gian mới`[l,r]`, nó có thể được thêm vào một chuỗi một cách chính xác khi`l >= end[chain]`. Nếu cả hai chuỗi đều không có sẵn, thì khoảng hiện tại sẽ trùng lặp với các khoảng đã chiếm cả hai chuỗi, do đó ba khoảng trùng nhau tại một điểm chung và không có giải pháp nào tồn tại. 

Sau khi có được hai chuỗi, vẫn còn một vấn đề tế nhị. Chúng ta cần nối chuỗi tăng với chuỗi giảm ở đỉnh. Chúng tôi giải quyết vấn đề này bằng cách buộc khoảng thời gian có điểm cuối bên phải lớn nhất toàn cầu thuộc về chuỗi giảm dần. Khi đó, khoảng đầu tiên của phía giảm, khi được sắp xếp theo điểm cuối bên phải giảm dần, có điểm cuối ít nhất bằng khoảng cuối cùng của phía tăng. Điều này làm cho quá trình chuyển đổi qua đỉnh có giá trị. 

Nếu màu tham lam đặt khoảng lớn nhất toàn cầu trong chuỗi đầu tiên, chỉ cần hoán đổi hai nhãn chuỗi. Việc hoán đổi màu sắc sẽ bảo tồn đặc tính là mỗi chuỗi chỉ chứa các khoảng không chồng chéo. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * (n/2)! * 2^(n/2))`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng cặp bạn bè`(a,b)`vào một khoảng`[l,r]`, Ở đâu`l = min(a,b)`Và`r = max(a,b)`. Giữ chỉ số cặp ban đầu để chúng ta có thể xây dựng lại hai chiều cao của nó sau này. Thông tin duy nhất liên quan đến khả năng tương thích bên trong mặt đơn điệu là điểm cuối nhỏ hơn và lớn hơn. 
2. Tìm cặp có điểm cuối bên phải`r`là tối đa trên toàn cầu. Cặp này cuối cùng sẽ được đặt ở phía giảm dần. Việc chọn mức tối đa toàn cục rất hữu ích vì nó tự động chiếm ưu thế trong khoảng cuối cùng ở phía tăng ở đỉnh. 
3. Sắp xếp tất cả các khoảng theo điểm cuối bên trái của chúng. Duy trì`end[0]`Và`end[1]`, điểm cuối ngoài cùng bên phải của các khoảng cuối cùng hiện được gán cho hai chuỗi. Ban đầu cả hai chuỗi đều trống. 
4. Xử lý các khoảng được sắp xếp từ trái sang phải. Nếu điểm cuối bên trái hiện tại`l`thỏa mãn`l >= end[0]`, gán khoảng cho chuỗi`0`. Bằng không, nếu`l >= end[1]`, gán nó vào chuỗi`1`. Nếu không có điều kiện nào xảy ra, hãy báo cáo`-1`. 

Phép gán tham lam là hợp lệ vì các khoảng thời gian được xử lý bằng cách tăng điểm cuối bên trái. Khi cả hai chuỗi đều bị chặn, khoảng hiện tại chồng lên một khoảng trong mỗi chuỗi, do đó ba khoảng trùng nhau ở điểm cuối hiện tại bên trái. Không thể tồn tại sự phân chia thành hai chuỗi không chồng chéo nhau. 
5. Sau khi tất cả các khoảng được chỉ định, hãy kiểm tra màu của khoảng điểm cuối cực đại bên phải trên toàn cầu. Nếu nó thuộc chuỗi`0`, hoán đổi hai nhãn chuỗi cho mỗi khoảng thời gian. Điều này chỉ thay đổi phía nào của ngọn núi mà chuỗi đại diện, chứ không phải thực tế là các khoảng bên trong mỗi chuỗi là rời rạc. 
6. Sắp xếp chuỗi`0`bằng cách tăng điểm cuối bên trái và nối từng cặp như`(l,r)`. Vì các khoảng liên tiếp trong chuỗi này thỏa mãn`previous_r <= current_l`, chuỗi hoàn chỉnh được tạo ra bởi chuỗi này không giảm. 
7. Sắp xếp chuỗi`1`bằng cách giảm điểm cuối bên phải và nối từng cặp thành`(r,l)`. Vì các khoảng không chồng chéo nên các điểm cuối bên trái của chúng cũng được sắp xếp theo hướng ngược lại một cách thích hợp, do đó chuỗi này không tăng. 
8. Nối chuỗi tăng và chuỗi giảm. Giá trị cuối cùng của chuỗi tăng là điểm cuối lớn nhất của khoảng cuối cùng của nó. Giá trị đầu tiên của chuỗi giảm là điểm cuối lớn nhất trong số tất cả các khoảng trong chuỗi`1`. Vì điểm cuối bên phải lớn nhất toàn cầu đã được cố tình đưa vào chuỗi`1`, giá trị đầu tiên ở phía giảm ít nhất là giá trị cuối cùng ở phía tăng. Như vậy toàn bộ dãy có hình dạng ngọn núi như yêu cầu. 

### Tại sao nó hoạt động 

Điều bất biến trong quá trình gán tham lam là mọi chuỗi đã được xây dựng đều là một chuỗi hợp lệ gồm các khoảng không chồng chéo. Khi một khoảng mới được gán cho một chuỗi, điểm cuối bên trái của nó ít nhất là điểm cuối bên phải cuối cùng của chuỗi đó, do đó bất biến vẫn đúng. Nếu cả hai chuỗi đều từ chối một khoảng, thì sẽ có một khoảng từ mỗi chuỗi có điểm cuối bên phải ít nhất là điểm cuối bên trái hiện tại. Cùng với khoảng thời gian hiện tại, ba khoảng thời gian trùng nhau tại thời điểm đó, do đó không thể tồn tại phân vùng hai chuỗi. 

Sau phép chia, mỗi khoảng trong chuỗi tăng được viết từ nhỏ đến lớn và mọi khoảng trong chuỗi giảm từ lớn đến nhỏ. Thứ tự bên trong mỗi chuỗi đảm bảo tính đơn điệu. Điểm cuối bên phải lớn nhất toàn cầu được đặt trong chuỗi giảm dần, do đó giá trị đầu tiên của chuỗi đó ít nhất là mọi điểm cuối bên phải trong chuỗi tăng dần. Điều đó chứng tỏ sự chuyển đổi ở đỉnh cao cũng có cơ sở. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    m = n // 2

    intervals = []
    global_max_idx = -1
    global_max_r = -1

    for i in range(m):
        a, b = map(int, input().split())
        l = min(a, b)
        r = max(a, b)
        intervals.append((l, r, i))

        if r > global_max_r:
            global_max_r = r
            global_max_idx = i

    intervals.sort()

    # end[c] is the right endpoint of the last interval
    # assigned to chain c.
    end = [-1, -1]
    color = [-1] * m

    for l, r, idx in intervals:
        if l >= end[0]:
            color[idx] = 0
            end[0] = r
        elif l >= end[1]:
            color[idx] = 1
            end[1] = r
        else:
            print(-1)
            return

    # The globally largest right endpoint must be on the
    # decreasing side. If it is currently on chain 0,
    # swap the two chain labels.
    if color[global_max_idx] == 0:
        for i in range(m):
            color[i] ^= 1

    left = []
    right = []

    for l, r, idx in intervals:
        if color[idx] == 0:
            left.append((l, r, idx))
        else:
            right.append((l, r, idx))

    left.sort(key=lambda x: (x[0], x[1]))
    right.sort(key=lambda x: (-x[1], -x[0]))

    ans = []

    for l, r, idx in left:
        ans.append(l)
        ans.append(r)

    for l, r, idx in right:
        ans.append(r)
        ans.append(l)

    print(*ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đầu tiên chuẩn hóa mỗi cặp thành`(l,r)`. Chỉ mục ban đầu được giữ lại vì hai phép gán chuỗi được thực hiện bằng khoảng thời gian chuẩn hóa, trong khi đầu ra cuối cùng chỉ cần hai độ cao ban đầu. Vì cặp này có thể được in theo một trong hai thứ tự nên việc lưu trữ`l`Và`r`là đủ. 

Các khoảng được sắp xếp theo`l`. hai`end`các giá trị đại diện cho khoảng thời gian cuối cùng hiện tại trong mỗi chuỗi. Sự so sánh là`l >= end[c]`, không`l > end[c]`, vì chiều cao liền kề bằng nhau là hợp pháp. Giá trị ban đầu`-1`hoạt động vì mọi chiều cao ít nhất`1`. 

Khi cả hai chuỗi đều không chấp nhận một khoảng thời gian thì không có giải pháp khả thi nào nên thuật toán có thể kết thúc ngay lập tức. Không cần phải quay lại. 

Điểm cuối bên phải tối đa toàn cầu bị buộc vào chuỗi`1`. Nếu nó được gán cho chuỗi`0`, lật từng màu là đủ. Điều này đơn giản hơn việc sửa đổi quy trình tham lam để buộc một khoảng thời gian cụ thể trong quá trình quét. 

Chuỗi bên trái được sắp xếp theo thứ tự tăng dần`l`. Bởi vì bất biến tham lam đảm bảo rằng mỗi khoảng bắt đầu không sớm hơn điểm kết thúc của khoảng trước đó, viết mỗi cặp là`l,r`tạo thành dãy không giảm. Chuỗi bên phải được sắp xếp theo thứ tự giảm dần`r`và mỗi cặp được viết là`r,l`, tạo ra dãy không tăng. 

Số nguyên Python có độ chính xác tùy ý, do đó giới hạn chiều cao của`10^9`không cần loại số nguyên đặc biệt. Mối quan tâm triển khai chính là bộ nhớ: thuật toán lưu trữ`O(n)`bộ dữ liệu và câu trả lời cuối cùng, phù hợp thoải mái trong giới hạn 256 MB cho`n <= 3 * 10^5`. 

## Ví dụ đã hoạt động 

Dấu vết đầu tiên sử dụng mẫu được cung cấp.```
8
1 3
4 2
6 7
5 7
```Sau khi chuẩn hóa, các khoảng thời gian là`[1,3]`,`[2,4]`,`[6,7]`, Và`[5,7]`. Chúng đã gần đạt đến thứ tự được sắp xếp, vì vậy quá trình tham lam rất dễ thực hiện. 

| Khoảng thời gian | Kết thúc hiện tại[0] | Kết thúc hiện tại[1] | Chuỗi được chọn | 
| --- | --- | --- | --- | 
|`[1,3]`|`-1`|`-1`|`0`| 
|`[2,4]`|`3`|`-1`|`1`| 
|`[5,7]`|`3`|`4`|`0`| 
|`[6,7]`|`7`|`4`|`1`| 

Điểm cuối bên phải tối đa toàn cầu là`7`và một trong các khoảng có điểm cuối này đã nằm trong chuỗi`1`. Chúng ta có thể giữ màu sắc như cũ. Xích`0`cho`1 3 5 7`, trong khi chuỗi`1`, được sắp xếp theo điểm cuối bên phải giảm dần, cho`7 6 4 2`. Trình tự cuối cùng là```
1 3 5 7 7 6 4 2
```Nó khác với đầu ra mẫu, điều này được cho phép vì bài toán chấp nhận bất kỳ sự sắp xếp hợp lệ nào. Đầu tiên nó tăng rồi giảm, và mọi cặp ban đầu vẫn liền kề nhau. 

Đối với dấu vết thứ hai, hãy xem xét thông tin đầu vào hợp lệ này:```
6
1 2
2 4
3 5
```Các khoảng là`[1,2]`,`[2,4]`, Và`[3,5]`. 

| Khoảng thời gian | Kết thúc hiện tại[0] | Kết thúc hiện tại[1] | Chuỗi được chọn | 
| --- | --- | --- | --- | 
|`[1,2]`|`-1`|`-1`|`0`| 
|`[2,4]`|`2`|`-1`|`0`| 
|`[3,5]`|`4`|`-1`|`1`| 

Khoảng thời gian`[3,5]`không thể tham gia chuỗi`0`bởi vì`3 < 4`, vì vậy nó đi vào chuỗi`1`. Điểm cuối bên phải tối đa toàn cầu là`5`, đã có trên chuỗi`1`. 

Xích`0`sản xuất`1 2 2 4`. Xích`1`sản xuất`5 3`. Kết quả cuối cùng là```
1 2 2 4 5 3
```Trình tự tăng lên thông qua`1,2,2,4,5`rồi giảm dần đến`3`. Dấu vết này cũng chứng minh tại sao các khoảng thời gian chạm lại tương thích:`[1,2]`Và`[2,4]`có thể chia sẻ chuỗi`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| có`n/2`khoảng thời gian, việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính. | 
| Không gian |`O(n)`| Các khoảng, phép gán chuỗi và mảng đầu ra đều sử dụng bộ nhớ tuyến tính. | 

Với nhiều nhất`150000`theo cặp, việc sắp xếp chỉ cần vài triệu thao tác ở mức so sánh và công việc còn lại là tuyến tính. Việc sử dụng bộ nhớ cũng tuyến tính theo số lượng học sinh, do đó giải pháp phù hợp thoải mái trong giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, do đó, bộ khai thác kiểm tra sẽ xác thực cách sắp xếp được trả về thay vì so sánh nó với một chuỗi chính xác. Trình trợ giúp bên dưới kiểm tra xem mọi cặp đầu vào có còn liền kề hay không, đầu ra có chứa chính xác độ cao được cung cấp hay không và chuỗi đầu tiên là không giảm, sau đó là không tăng.```python
import sys
import io

def solve_case(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = n // 2

    intervals = []
    global_max_idx = -1
    global_max_r = -1

    for i in range(m):
        a = next(it)
        b = next(it)
        l = min(a, b)
        r = max(a, b)
        intervals.append((l, r, i))

        if r > global_max_r:
            global_max_r = r
            global_max_idx = i

    intervals.sort()

    end = [-1, -1]
    color = [-1] * m

    for l, r, idx in intervals:
        if l >= end[0]:
            color[idx] = 0
            end[0] = r
        elif l >= end[1]:
            color[idx] = 1
            end[1] = r
        else:
            return "-1\n"

    if color[global_max_idx] == 0:
        for i in range(m):
            color[i] ^= 1

    left = []
    right = []

    for l, r, idx in intervals:
        if color[idx] == 0:
            left.append((l, r, idx))
        else:
            right.append((l, r, idx))

    left.sort(key=lambda x: (x[0], x[1]))
    right.sort(key=lambda x: (-x[1], -x[0]))

    ans = []

    for l, r, idx in left:
        ans.extend((l, r))

    for l, r, idx in right:
        ans.extend((r, l))

    return " ".join(map(str, ans)) + "\n"

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n = data[0]
    pairs = [tuple(sorted(data[i:i + 2])) for i in range(1, len(data), 2)]

    if out.strip() == "-1":
        # Verify that the instance really has no solution by
        # checking the same two-chain condition.
        intervals = [(a, b) for a, b in pairs]
        intervals.sort()

        end = [-1, -1]

        for l, r in intervals:
            if l >= end[0]:
                end[0] = r
            elif l >= end[1]:
                end[1] = r
            else:
                return True

        return False

    ans = list(map(int, out.split()))

    if len(ans) != n:
        return False

    expected = sorted(x for pair in pairs for x in pair)
    if sorted(ans) != expected:
        return False

    # Every original pair must appear as two consecutive values.
    remaining = pairs[:]
    used = [False] * len(remaining)

    for i in range(0, n, 2):
        cur = tuple(sorted((ans[i], ans[i + 1])))

        found = False
        for j, pair in enumerate(remaining):
            if not used[j] and pair == cur:
                used[j] = True
                found = True
                break

        if not found:
            return False

    # Check mountain property.
    phase = 0
    for i in range(1, n):
        if phase == 0:
            if ans[i] < ans[i - 1]:
                phase = 1
        else:
            if ans[i] > ans[i - 1]:
                return False

    return True

def run(inp: str) -> str:
    return solve_case(inp)

sample1 = """\
8
1 3
4 2
6 7
5 7
"""

sample2 = """\
6
1 2
2 4
3 5
"""

assert validate(sample1, run(sample1)), "sample 1"
assert validate(sample2, run(sample2)), "sample 2"

# Minimum size.
case_min = """\
2
10 3
"""
assert validate(case_min, run(case_min)), "minimum-size case"

# All heights equal.
case_equal = """\
8
7 7
7 7
7 7
7 7
"""
assert validate(case_equal, run(case_equal)), "all-equal case"

# Touching intervals must be accepted.
case_touching = """\
6
1 2
2 3
3 4
"""
assert validate(case_touching, run(case_touching)), "touching intervals"

# Three mutually overlapping intervals, so no two-chain partition exists.
case_impossible = """\
6
1 5
2 4
3 6
"""
assert validate(case_impossible, run(case_impossible)), "impossible overlap case"

# Maximum-size stress test.
m = 150000
parts = [str(2 * m)]
for i in range(1, m + 1):
    parts.append(f"{i} {i + 1}")
case_max = "\n".join(parts) + "\n"

result = run(case_max)
assert validate(case_max, result), "maximum-size case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ sự sắp xếp núi hợp lệ nào | Xây dựng cơ bản với hai chuỗi không cần thiết | 
|`6 / 1 2 / 2 4 / 3 5`| Bất kỳ sự sắp xếp núi hợp lệ nào | Đỉnh được hình thành bằng cách chuyển từ chuỗi này sang chuỗi khác | 
|`2 / 10 3`|`3 10`hoặc`10 3`| Đầu vào tối thiểu có thể và một cặp duy nhất | 
| Bốn cặp`7 7`| Tám`7`s | Điểm cuối bằng nhau và độ cao lặp lại bằng nhau | 
|`1 2`,`2 3`,`3 4`| Bất kỳ sự sắp xếp hợp lệ nào | Sử dụng đúng`>=`cho khoảng thời gian chạm | 
|`1 5`,`2 4`,`3 6`|`-1`| Ba khoảng chồng chéo đòi hỏi nhiều hơn hai chuỗi | 
|`150000`cặp`(i, i+1)`| Bất kỳ sự sắp xếp hợp lệ nào | Kích thước đầu vào tối đa và`O(n log n)`hiệu suất | 

## Vỏ cạnh 

Đối với khoảng thời gian chạm, hãy xem xét```
6
1 2
2 3
3 4
```Khoảng thời gian chuẩn hóa là`[1,2]`,`[2,3]`, Và`[3,4]`. Quá trình quét tham lam có thể đặt cả ba vào cùng một chuỗi vì mỗi điểm cuối bên trái mới chính xác là điểm cuối bên phải trước đó. Dãy số tăng dần thu được là`1 2 2 3 3 4`, hợp lệ. Việc sử dụng`l >= end`chính xác là điều khiến việc này thành công. 

Đối với một đỉnh bên trong một cặp, hãy xem xét```
4
1 4
2 3
```Các khoảng chồng lên nhau, nhưng chỉ có hai khoảng, vì vậy chúng có thể được đặt ở các phía đối diện nhau. Nhiệm vụ tham lam đặt`[1,4]`Và`[2,3]`thành các chuỗi khác nhau. Khoảng có điểm cuối bên phải tối đa là`[1,4]`, do đó chuỗi của nó trở thành phía giảm. Chuỗi còn lại sản xuất`2 3`, Và`[1,4]`được viết là`4 1`, cho`2 3 4 1`. Quá trình chuyển đổi vẫn hợp lệ ngay cả khi các khoảng thời gian đó trùng nhau. 

Đối với ba khoảng chồng chéo, hãy xem xét```
6
1 5
2 4
3 6
```Sau khi sắp xếp,`[1,5]`chiếm chuỗi đầu tiên và`[2,4]`chiếm thứ hai. Khi`[3,6]`được xử lý,`3 < 5`Và`3 < 4`, vì vậy không có chuỗi nào có sẵn. Ở độ cao`3`, cả ba khoảng đều trùng nhau. Vì một ngọn núi hợp lệ chỉ có một bên tăng và một bên giảm nên ít nhất hai trong số các khoảng này sẽ phải chia sẻ một bên, điều này là không thể. Thuật toán in chính xác`-1`. 

Đối với các cặp bằng nhau, hãy xem xét```
4
3 3
3 3
```Cả hai khoảng đều`[3,3]`. Người đầu tiên có thể vào chuỗi`0`, trong khi thứ hai có thể vào chuỗi`1`bởi vì`3 >= 3`. Sau khi xây dựng, cả hai cặp sản xuất`3 3`, và trình tự cuối cùng là`3 3 3 3`. Điều này chứng tỏ rằng các điểm cuối bằng nhau và chiều cao cặp bằng nhau không yêu cầu trường hợp đặc biệt nào ngoài việc sử dụng các so sánh không nghiêm ngặt. 

Đối với kích thước đầu vào tối đa, trường hợp ứng suất được tạo chứa`150000`cặp hình thức`(i, i+1)`. Mỗi khoảng có thể theo sau khoảng trước vì điểm cuối bên trái của nó bằng điểm cuối bên phải trước đó. Quá trình quét tham lam sẽ gán chúng một cách hiệu quả mà không cần quay lại và hai thao tác sắp xếp vẫn được giữ nguyên.`O(n log n)`. Đây là thang đo được yêu cầu ban đầu`n <= 3 * 10^5`hạn chế.
