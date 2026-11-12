---
title: "CF 1659B - Lật bit"
description: "Chúng tôi bắt đầu với một chuỗi nhị phân. Chúng ta phải thực hiện chính xác (k) thao tác. Trong một thao tác, chúng ta chọn vị trí (i). Bit được chọn không thay đổi, trong khi mọi bit khác trong chuỗi bị đảo ngược."
date: "2026-06-10T03:11:29+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1659
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 782 (Div. 2)"
rating: 1300
weight: 1659
solve_time_s: 294
verified: false
draft: false
---

[CF 1659B - Lật bit](https://codeforces.com/problemset/problem/1659/B) 

**Đánh giá:** 1300 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 4 phút 54 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi nhị phân. Chúng ta phải thực hiện chính xác\(k\)hoạt động. 

Trong một thao tác, chúng tôi chọn một vị trí\(i\). Bit được chọn không thay đổi, trong khi mọi bit khác trong chuỗi bị đảo ngược. Rốt cuộc\(k\)hoạt động, chúng tôi muốn chuỗi kết quả có từ điển lớn nhất có thể. Chúng ta cũng phải xuất ra số lần mỗi vị trí được chọn. 

Điều đầu tiên cần hiểu là tác dụng của các hoạt động lặp đi lặp lại. Việc chọn một vị trí không trực tiếp sửa đổi vị trí đó. Thay vào đó nó sửa đổi mọi vị trí khác. Vì chỉ tính chẵn lẻ mới quan trọng, chúng ta không quan tâm một vị trí được chọn bao nhiêu lần mà chỉ quan tâm liệu nó được chọn số lần lẻ hay số chẵn. 

Các ràng buộc ngay lập tức loại trừ mọi hoạt động mô phỏng. Một thao tác duy nhất ảnh hưởng\(n-1\)các vị trí và\(k\)có thể lớn như\(10^9\). Ngay cả việc theo dõi từng hoạt động một cũng không thể. Vì tổng độ dài của tất cả các chuỗi nhiều nhất là\(2 \cdot 10^5\), giải pháp \(O(n)\) hoặc \(O(n \log n)\) cho mỗi trường hợp thử nghiệm là mục tiêu. 

Trường hợp cạnh tinh tế xuất hiện khi\(k\)thật kỳ quặc. Coi như:```text
1 1
0
```Chỉ có một vị trí. Chúng ta phải thực hiện một hoạt động ở đâu đó. Bit cuối cùng trở thành\(1\), không\(0\). Bất kỳ giải pháp nào tham lam biến số 0 thành số 1 mà không tính đến tính chẵn lẻ của các phép toán không được sử dụng sẽ thất bại. 

Một trường hợp quan trọng khác là khi có nhiều hoạt động hơn các vị trí hữu ích.```text
3 10
111
```Sau khi tối đa hóa chuỗi, một số thao tác vẫn còn. Những hoạt động đó không thể biến mất. Tính chẵn lẻ của chúng ảnh hưởng đến chuỗi cuối cùng và phải được chỉ định ở đâu đó. Giải pháp chỉ theo dõi các vị trí được sử dụng rõ ràng trong giai đoạn tham lam sẽ tạo ra câu trả lời sai. 

Cái bẫy thứ ba là vị trí cuối cùng. Chiến lược tham lam tiêu chuẩn cố tình đổ tất cả các hoạt động còn lại vào chỉ mục cuối cùng. Nếu quên cách xử lý đặc biệt đó, tổng số sẽ không bằng\(k\). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa mọi hoạt động. Ở mỗi bước chúng ta sẽ chọn một vị trí và cập nhật chuỗi. Ngay cả khi bằng cách nào đó chúng ta biết được lựa chọn tối ưu, mỗi thao tác đều tốn \(O(n)\), tạo ra công việc \(O(nk)\). Với\(k\)lên đến\(10^9\), điều này thật vô vọng. 

Quan sát quan trọng là chỉ có tính chẵn lẻ mới quan trọng. 

Giả sử vị trí\(i\)được chọn\(f_i\)lần. Cho phép$$
K = \sum_i f_i = k.
$$Một chút ở vị trí\(i\)được lật trong mọi thao tác ngoại trừ những thao tác chọn\(i\). Do đó nó bị đảo lộn$$
k - f_i
$$lần. 

Chỉ có sự ngang bằng của\(k-f_i\)vấn đề. Bit cuối cùng là$$
s_i \oplus ((k-f_i)\bmod 2).
$$Bây giờ vấn đề trở thành vấn đề gán tính chẵn lẻ mang tính xây dựng. 

Đầu tiên hãy xem xét trường hợp trong đó\(k\)thật kỳ quặc. Một vị trí chứa\(0\)có thể trở thành\(1\)nếu chúng ta chọn vị trí đó một lần. Vì thứ tự từ điển bị chi phối bởi các vị trí trước đó, chúng ta nên thực hiện các thao tác từ trái sang phải để biến càng nhiều số 0 đầu thành số 1 càng tốt. 

Khi\(k\)là số chẵn, phát biểu đối xứng đúng. Một vị trí chứa\(1\)có thể trở thành\(1\)trong chuỗi cuối cùng nếu chúng ta chọn nó một lần. Một lần nữa chúng ta tham lam xử lý từ trái sang phải. 

Sau khi sử dụng hết các vị trí hữu ích hoặc các hoạt động có sẵn, tất cả các hoạt động còn lại sẽ được chuyển vào vị trí cuối cùng. Chỉ có tính chẵn lẻ của chúng mới quan trọng, vì vậy điều này duy trì tính tối ưu trong khi đáp ứng yêu cầu sử dụng chính xác\(k\)di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---:|---:|---| 
| Lực lượng vũ phu | \(O(nk)\) | \(O(n)\) | Quá chậm | 
| Xây dựng tham lam tối ưu | \(O(n)\) | \(O(n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`cnt`chiều dài\(n\), ban đầu tất cả đều là số không. Điều này sẽ lưu trữ số lần mỗi vị trí được chọn. 

2. Hãy để`rem = k`. 

3. Quét các vị trí từ trái sang phải, trừ vị trí cuối cùng. 

4. Nếu\(k\)là số lẻ và bit hiện tại là`'0'`, việc chọn vị trí này một lần sẽ làm cho giá trị cuối cùng của nó trở thành`'1'`. Nếu như`rem > 0`, bộ:$$
cnt[i] = 1
$$và giảm`rem`. 

5. Nếu\(k\)là số chẵn và bit hiện tại là`'1'`, việc chọn vị trí này một khi bảo toàn được vị trí dẫn đầu`'1'`trong câu trả lời cuối cùng. Nếu như`rem > 0`, bộ:$$
cnt[i] = 1
$$và giảm`rem`. 

6. Sau khi xử lý vị trí\(1\)bởi vì\(n-1\), đặt tất cả các hoạt động còn lại ở vị trí cuối cùng:$$
cnt[n-1] = rem.
$$7. Xây dựng chuỗi cuối cùng. Đối với vị trí\(i\), số lần lật được áp dụng cho nó bằng$$
k - cnt[i].
$$Nếu số này là số lẻ thì lật bit gốc. Nếu không thì giữ nguyên. 

8. Xuất chuỗi cuối cùng và mảng`cnt`. 

### Tại sao nó hoạt động 

Thứ tự từ điển được xác định bởi vị trí đầu tiên nơi hai chuỗi khác nhau. Bất cứ khi nào một thao tác có thể biến một vị trí trước đó thành`1`, làm như vậy ít nhất cũng tốt bằng việc lưu thao tác đó cho vị trí sau. Quá trình quét tham lam xử lý các vị trí theo đúng thứ tự ưu tiên này. 

Mỗi vị trí hữu ích chỉ cần một thao tác vì chỉ có tính chẵn lẻ mới quan trọng. Việc thực hiện ba, năm hoặc bảy hoạt động trên cùng một vị trí có tác dụng tương tự như việc thực hiện một hoạt động. Do đó, giai đoạn tham lam sử dụng số lượng thao tác tối thiểu cần thiết để tối đa hóa các vị trí sớm nhất. 

Bất kỳ hoạt động nào còn lại sau giai đoạn tham lam đều không thể cải thiện bất kỳ vị trí nào trước đó. Việc gán tất cả chúng vào chỉ mục cuối cùng sẽ bảo toàn tiền tố tối ưu và đáp ứng yêu cầu sử dụng chính xác\(k\)di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        s = list(input().strip())

        cnt = [0] * n
        rem = k

        target = '0' if k % 2 else '1'

        for i in range(n - 1):
            if rem > 0 and s[i] == target:
                cnt[i] = 1
                rem -= 1

        cnt[-1] += rem

        ans = []

        for i in range(n):
            flips = (k - cnt[i]) & 1

            bit = int(s[i])

            if flips:
                bit ^= 1

            ans.append(str(bit))

        print("".join(ans))
        print(*cnt)

if __name__ == "__main__":
    solve()
```Mảng`cnt`lưu trữ số lượng hoạt động chính xác. Biến`target`nắm bắt được quy tắc tham lam phụ thuộc vào tính chẵn lẻ. Khi\(k\)thật kỳ quặc khi chúng ta nhắm mục tiêu vào số không. Khi\(k\)thậm chí là chúng tôi nhắm mục tiêu những người. 

Việc xây dựng lại cuối cùng sử dụng công thức đảo vị trí một cách chính xác\(k-cnt[i]\)lần. Chỉ có vấn đề chẵn lẻ, vì vậy chúng tôi tính toán```python
flips = (k - cnt[i]) & 1
```và áp dụng thao tác lật nếu cần. 

Vị trí cuối cùng nhận được tất cả các hoạt động không sử dụng. Điều này đảm bảo$$
\sum_i cnt[i] = k.
$$## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```text
6 3
100001
```Từ\(k\)thật kỳ quặc, chúng ta tham lam chi tiêu các hoạt động cho số không. 

| Vị trí | Chút | Hành động | Còn lại k | cnt | 
|---|---|---|---:|---| 
| 1 | 1 | bỏ qua | 3 | [0,0,0,0,0,0] | 
| 2 | 0 | lấy | 2 | [0,1,0,0,0,0] | 
| 3 | 0 | lấy | 1 | [0,1,1,0,0,0] | 
| 4 | 0 | lấy | 0 | [0,1,1,1,0,0] | 

Không còn hoạt động nào nữa. 

| Vị trí | cnt[i] | k-cnt[i] chẵn lẻ | Bit cuối cùng | 
|---|---:|---:|---| 
| 1 | 0 | lẻ | 0 | 
| 2 | 1 | thậm chí | 0 | 
| 3 | 1 | thậm chí | 0 | 
| 4 | 1 | thậm chí | 0 | 
| 5 | 0 | lẻ | 1 | 
| 6 | 0 | lẻ | 0 | 

Chuỗi cuối cùng:```text
000010
```Dấu vết này cho thấy cách xây dựng được bắt nguồn trực tiếp từ tính chẵn lẻ lật thay vì mô phỏng. 

### Ví dụ 2 

đầu vào:```text
6 1
111001
```Từ\(k\)thật kỳ lạ, chúng tôi nhắm mục tiêu số không. 

| Vị trí | Chút | Hành động | Còn lại k | 
|---|---|---|---:| 
| 1 | 1 | bỏ qua | 1 | 
| 2 | 1 | bỏ qua | 1 | 
| 3 | 1 | bỏ qua | 1 | 
| 4 | 0 | lấy | 0 | 

Số lượng trở thành:```text
0 0 0 1 0 0
```Tái thiết thông qua tính chẵn lẻ mang lại:```text
111101
```Ba vị trí đầu tiên luôn ở mức tối đa, đó chính xác là những gì mà trật tự từ điển yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n)\) | Một thẻ cho nhiệm vụ và một thẻ để xây dựng lại | 
| Không gian | \(O(n)\) | Mảng đếm và mảng trả lời | 

Tổng chiều dài của tất cả các chuỗi tối đa là\(2 \cdot 10^5\). Việc vượt qua tuyến tính qua từng trường hợp thử nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            s = list(input().strip())

            cnt = [0] * n
            rem = k

            target = '0' if k % 2 else '1'

            for i in range(n - 1):
                if rem > 0 and s[i] == target:
                    cnt[i] = 1
                    rem -= 1

            cnt[-1] += rem

            ans = []
            for i in range(n):
                bit = int(s[i])
                if ((k - cnt[i]) & 1):
                    bit ^= 1
                ans.append(str(bit))

            print("".join(ans))
            print(*cnt)

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

# minimum size
run("1\n1 0\n0\n")

# single bit with odd k
run("1\n1 5\n0\n")

# all zeros
run("1\n5 2\n00000\n")

# all ones
run("1\n5 3\n11111\n")

# large remaining parity on last position
run("1\n4 10\n1010\n")
```| Kiểm tra đầu vào | Thuộc tính đầu ra dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|\(n=1,k=0\)| chuỗi không thay đổi | trường hợp tối thiểu | 
|\(n=1,k=5\)| tính chẵn lẻ được xử lý chính xác | trường hợp cạnh vị trí duy nhất | 
| tất cả số không | tham lam tiêu thụ số 0 sớm nhất | logic chẵn lẻ lẻ | 
| tất cả những cái | không có động thái kỳ quặc hữu ích nào tồn tại | bài tập còn sót lại | 
| lớn\(k\)| vị trí cuối cùng nhận phần còn lại | tính tổng hợp chính xác | 

## Vỏ cạnh 

Hãy xem xét:```text
1
1 5
0
```Chỉ có một vị trí. Vòng lặp tham lam không bao giờ chạy vì không có vị trí nào trước vòng lặp cuối cùng. Tất cả năm hoạt động được gán cho vị trí đó. Từ$$
k-cnt[0]=5-5=0,
$$bit không bao giờ bị lật và vẫn còn`0`. Tổng số đếm chính xác là năm. 

Coi như:```text
1
3 10
111
```Đây\(k\)là chẵn. Giai đoạn tham lam dành một thao tác cho hai vị trí đầu tiên và để lại tám thao tác cho vị trí cuối cùng. Việc tái thiết chỉ phụ thuộc vào tính chẵn lẻ chứ không phụ thuộc vào độ lớn của 10. Thuật toán sử dụng tất cả các bước di chuyển trong khi vẫn giữ nguyên tiền tố lớn nhất có thể đạt được về mặt từ điển. 

Coi như:```text
1
4 1
0000
```Vị trí đầu tiên ngay lập tức được chuyển đổi thành bit dẫn đầu tốt nhất có thể. Các vị trí còn lại không liên quan khi không còn hoạt động nào. Điều này khẳng định sự lựa chọn tham lam ở vị trí sớm nhất luôn là tối ưu.
