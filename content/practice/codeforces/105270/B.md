---
title: "CF 105270B - MEX tối thiểu"
description: "Chúng tôi được cung cấp một mảng và chúng tôi được phép xem xét bất kỳ phân đoạn liền kề nào của nó. Đối với mỗi phân đoạn như vậy, chúng tôi tính toán MEX của nó, số nguyên không âm nhỏ nhất không xuất hiện bên trong phân đoạn đó. Trong số tất cả các phân khúc, trước tiên chúng tôi quan tâm đến những phân khúc có MEX càng lớn càng tốt."
date: "2026-06-23T13:03:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105270
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #32 (2^5-Forces, TheForces Rated, Prizes!)"
rating: 0
weight: 105270
solve_time_s: 87
verified: false
draft: false
---

[CF 105270B - MEX tối thiểu](https://codeforces.com/problemset/problem/105270/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi được phép xem xét bất kỳ phân đoạn liền kề nào của nó. Đối với mỗi phân đoạn như vậy, chúng tôi tính toán MEX của nó, số nguyên không âm nhỏ nhất không xuất hiện bên trong phân đoạn đó. Trong số tất cả các phân khúc, trước tiên chúng tôi quan tâm đến những phân khúc có MEX càng lớn càng tốt. Sau khi xác định giá trị MEX tối đa có thể đạt được, chúng tôi xem xét tất cả các phân đoạn đạt được giá trị đó và chọn phân đoạn có độ dài nhỏ nhất. Nhiệm vụ là chỉ xuất ra độ dài tối thiểu đó. 

Một cách hữu ích để diễn giải lại điều này là suy nghĩ xem việc một phân khúc có MEX lớn có ý nghĩa như thế nào. Nếu một phân đoạn có MEX bằng k thì mọi giá trị từ 0 đến k−1 phải xuất hiện ít nhất một lần bên trong phân đoạn đó, trong khi bản thân k phải vắng mặt. Vì vậy, tối đa hóa MEX có nghĩa là chúng tôi muốn một phân đoạn chứa càng nhiều số nguyên nhỏ riêng biệt bắt đầu từ 0 càng tốt, mà không bao gồm số nguyên bị thiếu đầu tiên. 

Các ràng buộc ngụ ý rằng chúng tôi không thể thử tất cả các phân đoạn O(n2). Với tổng n lên tới 10⁵ trong các trường hợp thử nghiệm, ngay cả O(n√n) cũng không an toàn. Chúng tôi cần một cái gì đó tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi có sự trùng lặp. Ví dụ, nếu mảng là`[0, 1, 0, 1, 2]`, người ta dễ nghĩ rằng các phân đoạn dài hơn thì tốt hơn vì chúng “tích lũy” nhiều giá trị hơn, nhưng MEX chỉ phụ thuộc vào sự hiện diện chứ không phải tần suất. Bất kỳ cách tiếp cận nào theo dõi số lượng không chính xác hoặc cố gắng mở rộng một cách tham lam mà không kiểm tra các giá trị bị thiếu đều có thể đánh giá quá cao chất lượng phân khúc. 

Một trường hợp phức tạp khác là khi thiếu hoàn toàn số 0. Khi đó, mọi phân đoạn đều có MEX 0, do đó MEX tối đa là 0 và câu trả lời sẽ trở thành phân đoạn ngắn nhất có thể, là 1. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê từng cặp (l, r), tính MEX của phân khúc đó và theo dõi kết quả tốt nhất. Bản thân việc tính toán MEX sẽ mất O(r−l+1) nếu được thực hiện bằng cách quét theo tập hợp, do đó cách tiếp cận đầy đủ sẽ trở thành O(n³) trong trường hợp xấu nhất. Ngay cả khi được tối ưu hóa bằng mảng tần số và các bản cập nhật gia tăng, chúng tôi vẫn có các phân đoạn O(n²) cần xem xét, phân đoạn này quá lớn đối với n tối đa 10⁵. 

Quan sát quan trọng là MEX của một đoạn chỉ phụ thuộc vào việc liệu tất cả các số từ 0 đến k−1 nào đó có xuất hiện bên trong nó hay không. Vì vậy, thay vì nghĩ về các phân đoạn tùy ý, chúng tôi tập trung vào những giá trị mà chúng tôi đang cố gắng đưa vào. 

Gọi M là MEX toàn cục của toàn bộ mảng. Không có phân đoạn nào có thể có MEX lớn hơn M, vì M không xuất hiện ở mọi nơi. Do đó MEX tối đa có thể chính xác là M. Vì vậy, chúng tôi chỉ cần các phân đoạn chứa mọi số trong`[0, M−1]`. 

Bây giờ vấn đề trở thành: trong số tất cả các phân đoạn bao gồm ít nhất một lần xuất hiện của mọi giá trị từ 0 đến M−1, hãy tìm phân đoạn ngắn nhất. Đây là một vấn đề cổ điển về “cửa sổ tối thiểu bao gồm một tập hợp các phần tử bắt buộc”. Chúng ta có thể giải quyết nó bằng cách sử dụng cửa sổ trượt có tính tần số. 

Chúng tôi duy trì một cửa sổ [l, r] và mở rộng r trong khi theo dõi số lượng giá trị bắt buộc riêng biệt mà chúng tôi đã đề cập. Khi cửa sổ chứa tất cả các giá trị từ 0 đến M−1, chúng tôi thử thu nhỏ l để giảm thiểu độ dài trong khi vẫn duy trì vùng phủ sóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính toán MEX toàn cục của mảng. Điều này được thực hiện bằng cách đánh dấu những giá trị nào từ 0 trở lên xuất hiện và dừng ở giá trị còn thiếu đầu tiên. Đặt giá trị này là M. 

Sau đó, chúng tôi giải quyết vấn đề bao phủ trên tập hợp`{0, 1, ..., M−1}`. 

1. Xây dựng mảng tần số hoặc sơ đồ băm cho các giá trị trong cửa sổ hiện tại. Chúng tôi chỉ quan tâm đến các giá trị trong`[0, M−1]`, bỏ qua mọi thứ lớn hơn vì chúng không ảnh hưởng đến MEX. 

Lý do là MEX chỉ phụ thuộc vào việc thiếu giá trị thiếu nhỏ nhất. 
2. Duy trì bộ đếm`have`theo dõi số lượng giá trị bắt buộc riêng biệt hiện có trong cửa sổ. 
3. Mở rộng con trỏ bên phải`r`từng bước một. Đối với mỗi phần tử mới, nếu nó nằm trong`[0, M−1]`, tăng tần số của nó. Nếu nó xuất hiện lần đầu tiên, hãy tăng`have`. 
4. Khi nào`have == M`, cửa sổ hiện tại chứa tất cả các giá trị bắt buộc, vì vậy nó hợp lệ và có MEX ít nhất là M. Vì M là mức tối đa có thể nên cửa sổ này là tối ưu về mặt MEX. 
5. Lúc này, hãy thu nhỏ con trỏ bên trái`l`càng nhiều càng tốt trong khi vẫn giữ`have == M`. Mỗi lần chúng tôi di chuyển`l`, chúng tôi cập nhật tần số và có thể giảm`have`. 
6. Trong mỗi cửa sổ hợp lệ, hãy cập nhật câu trả lời với độ dài tối thiểu nhìn thấy. 
7. Tiếp tục cho đến khi con trỏ bên phải chạm đến cuối. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào,`have == M`khi và chỉ nếu cửa sổ hiện tại chứa ít nhất một lần xuất hiện của mọi số nguyên trong`[0, M−1]`. Điều kiện này hoàn toàn tương đương với cửa sổ có MEX bằng M. Vì không có phân đoạn nào có thể vượt quá MEX M nên mọi cửa sổ hợp lệ đều là một giải pháp ứng cử viên. Bước thu gọn đảm bảo rằng đối với mỗi điểm cuối bên phải, chúng ta chỉ xem xét điểm cuối bên trái hợp lệ nhỏ nhất nên không bỏ sót phân đoạn tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        seen = [False] * (n + 2)
        for x in a:
            if x <= n + 1:
                seen[x] = True

        mex = 0
        while seen[mex]:
            mex += 1

        if mex == 0:
            print(1)
            continue

        freq = [0] * (mex + 1)
        have = 0
        ans = n
        l = 0

        for r in range(n):
            x = a[r]
            if 0 <= x < mex:
                if freq[x] == 0:
                    have += 1
                freq[x] += 1

            while have == mex:
                ans = min(ans, r - l + 1)
                y = a[l]
                if 0 <= y < mex:
                    freq[y] -= 1
                    if freq[y] == 0:
                        have -= 1
                l += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán MEX toàn cầu bằng cách đánh dấu số nguyên nào xuất hiện. Điều này là cần thiết vì nó xác định tập hợp phạm vi mục tiêu. Cửa sổ trượt khi đó chỉ hoạt động trên các giá trị bên dưới MEX đó, vì các giá trị lớn hơn không liên quan. 

Vòng co lại là phần tinh tế nhất. điều kiện`while have == mex`đảm bảo chúng tôi liên tục giảm thiểu ranh giới bên trái cho từng ranh giới bên phải cố định, đây là điều đảm bảo độ dài phân đoạn tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = [0, 1, 0, 1, 2]`MEX toàn cầu là 3 vì 0, 1, 2 đều có mặt và 3 bị thiếu. 

Chúng ta cần mảng con ngắn nhất chứa {0,1,2}. 

| r | tôi | cửa sổ | trạng thái tần số | có | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0] | {0:1} | 1 | mở rộng | 
| 1 | 0 | [0,1] | {0,1} | 2 | mở rộng | 
| 2 | 0 | [0,1,0] | {0,1} | 2 | mở rộng | 
| 3 | 0 | [0,1,0,1] | {0,1} | 2 | mở rộng | 
| 4 | 0 | [0,1,0,1,2] | {0,1,2} | 3 | hợp lệ | 

Bây giờ chúng tôi thu nhỏ lại: 

| r | tôi | cửa sổ | có | hành động | 
| --- | --- | --- | --- | --- | 
| 4 | 0 | [0,1,0,1,2] | 3 | thu nhỏ l→1 | 
| 4 | 1 | [1,0,1,2] | 3 | co lại l→2 | 
| 4 | 2 | [0,1,2] | 3 | điểm dừng thu nhỏ | 

Độ dài tối thiểu là 3. 

Điều này chứng tỏ rằng sự trùng lặp không quan trọng, chỉ có mức độ phù hợp mới có. 

### Ví dụ 2 

đầu vào:`a = [1, 2, 3]`MEX toàn cầu là 0 vì thiếu 0. 

Bất kỳ phân đoạn nào cũng có MEX 0, vì vậy câu trả lời là 1. 

Thuật toán xử lý ngay việc này thông qua trường hợp đặc biệt`mex == 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần | 
| Không gian | O(n) | Mảng tần số và mảng nhìn thấy | 

Tổng n trên các thử nghiệm là 10⁵, do đó việc xử lý tuyến tính cho mỗi thử nghiệm là đủ và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        seen = [False] * (n + 2)
        for x in a:
            if x <= n + 1:
                seen[x] = True

        mex = 0
        while seen[mex]:
            mex += 1

        if mex == 0:
            out.append("1")
            continue

        freq = [0] * (mex + 1)
        have = 0
        ans = n
        l = 0

        for r in range(n):
            x = a[r]
            if 0 <= x < mex:
                if freq[x] == 0:
                    have += 1
                freq[x] += 1

            while have == mex:
                ans = min(ans, r - l + 1)
                y = a[l]
                if 0 <= y < mex:
                    freq[y] -= 1
                    if freq[y] == 0:
                        have -= 1
                l += 1

        out.append(str(ans))

    return "\n".join(out)

# provided sample (format adapted if needed)
assert run("""1
5
0 1 0 1 2
""") == "3"

# minimum size, mex 0
assert run("""1
3
1 2 3
""") == "1"

# all equal
assert run("""1
5
7 7 7 7 7
""") == "1"

# consecutive full coverage
assert run("""1
5
0 1 2 3 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảo hiểm đầy đủ duy nhất | 3 | thu nhỏ cửa sổ đúng cách | 
| không có số không | 1 | mex = 0 trường hợp | 
| tất cả đều bình đẳng | 1 | đoạn tối thiểu tầm thường | 
| bảo hiểm tiền tố đầy đủ | 3 | xử lý các giá trị lặp lại | 

## Vỏ cạnh 

Khi mảng không bao giờ chứa 0, MEX được tính toán là 0 và thuật toán trực tiếp đưa ra 1. Đối với đầu vào`[5, 6, 7]`, không cần xử lý tần số vì không có phân đoạn nào có thể cải thiện MEX vượt quá 0. 

Khi tất cả các giá trị bắt buộc xuất hiện nhiều lần, cửa sổ trượt phải bỏ qua các giá trị trùng lặp. Vì`[0,1,0,1,2]`, tần số logic đảm bảo`have`chỉ tính sự hiện diện rõ ràng, do đó việc thu nhỏ không phá vỡ tính chính xác. 

Khi các giá trị bắt buộc nằm cách xa nhau, cửa sổ sẽ mở rộng cho đến khi bao gồm giá trị bắt buộc cuối cùng, sau đó thu hẹp lại. Điều này đảm bảo rằng ngay cả khi đoạn tối ưu bắt đầu muộn thì nó vẫn được phát hiện khi con trỏ bên phải chạm tới điểm cuối của nó.
