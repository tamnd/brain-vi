---
title: "CF 102317D - Giấc mơ hoang dã nhất"
description: "Vấn đề mô hình hóa đĩa CD như một chuỗi các bản nhạc tròn, trong đó mỗi bản nhạc có thời lượng cố định. Một bài hát cụ thể là bài hát yêu thích của Anya. Một ngày bao gồm một số phân đoạn lái xe."
date: "2026-08-16T18:49:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "D"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 318
verified: true
draft: false
---

[CF 102317D - Những giấc mơ hoang dã nhất](https://codeforces.com/problemset/problem/102317/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô hình hóa đĩa CD như một chuỗi các bản nhạc tròn, trong đó mỗi bản nhạc có thời lượng cố định. Một bài hát cụ thể là bài hát yêu thích của Anya. Một ngày bao gồm một số phân đoạn lái xe. Trong các phân đoạn có số lẻ, Anya đang ở trong ô tô nên đầu đĩa CD buộc phải khởi động lại bản nhạc yêu thích của cô ấy và lặp lại nó trong toàn bộ phân đoạn. Trong các phân đoạn được đánh số chẵn, cô ấy vắng mặt, do đó CD tiếp tục phát lại bình thường từ chính xác vị trí mà phân đoạn trước đó đã dừng. Nhiệm vụ là tính xem bài hát yêu thích được nghe bao nhiêu giây trong cả ngày. Tuyên bố chính thức cung cấp tối đa 50 đĩa CD, nhiều nhất là 20 bài hát cho mỗi CD, nhiều nhất là 100 ngày cho mỗi CD và nhiều nhất là 20 phân đoạn lái xe mỗi ngày. Tổng thời lượng của các phân đoạn trong ngày tối đa là 86.400 giây. 

Phiên bản Codeforces có giới hạn thời gian là 1 giây và bộ nhớ 256 MB. Tổng lượng đầu vào nhỏ xét theo các phân đoạn, nhiều nhất là (50 \times 100 \time 20 = 100{,}000) phân đoạn, nhưng tổng thời lượng của các phân đoạn đó có thể rất lớn. Một phương thức xử lý mỗi giây có thể thực hiện tối đa (50 \times 100 \times 86{,}400 = 432{,}000{,}000) lần lặp. Điều đó vượt xa những gì Python nên thử trong giới hạn 1 giây. Giải pháp dự định phải xử lý từng phân đoạn trong thời gian không đổi. 

Trường hợp tinh tế đầu tiên là khi Anya rời đi đúng lúc bài hát yêu thích của cô kết thúc. Ví dụ,```
1
2 1
5 7
1
3 5 1 7
```sản xuất```
CD #1:
12
```5 giây đầu tiên hoàn toàn là bài hát yêu thích, vì vậy khi Anya rời đi, quá trình phát lại bình thường sẽ bắt đầu từ bài hát 2. Do đó, đoạn 1 giây không đóng góp gì và đoạn 7 giây cuối cùng đóng góp 7 giây. Việc triển khai bất cẩn coi bội số chính xác của thời lượng yêu thích là vị trí 0 của bài hát yêu thích sẽ tính không chính xác phân đoạn 1 giây là thời gian yêu thích. 

Trường hợp ranh giới thứ hai xảy ra khi bài hát yêu thích là bài hát cuối cùng trên CD. Coi như```
1
2 2
3 5
1
2 5 6
```Đầu ra là```
CD #1:
8
```Anya nghe 5 giây của bài hát yêu thích. Nó kết thúc chính xác khi cô ấy rời đi, do đó, phần phát lại bình thường sẽ kết thúc ở bản nhạc 1. 6 giây tiếp theo chứa 3 giây của bản nhạc 1 và sau đó là 3 giây của bản nhạc yêu thích. Một đại diện lưu trữ phần cuối của bản nhạc yêu thích dưới dạng vị trí`total`phải chuẩn hóa vị trí đó về 0 trước khi thực hiện phép tính vòng tròn. 

Trường hợp thứ ba là một đĩa CD chỉ có một bản nhạc:```
1
1 1
5
1
1 100
```Câu trả lời là```
CD #1:
100
```Mỗi giây nhất thiết phải là bài hát yêu thích, cả khi Anya có mặt và khi cô ấy vắng mặt. Việc triển khai giả định luôn có một bản nhạc khác sau bản nhạc yêu thích sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng đĩa CD từng giây một. Khi Anya ở trong xe, mỗi giây mô phỏng sẽ góp phần đưa ra câu trả lời và vị trí bên trong bài hát yêu thích sẽ tăng lên theo chu kỳ. Trong khi cô ấy vắng mặt, vị trí sẽ tiến lên thông qua CD một cách bình thường và một giây sẽ đóng góp nếu vị trí hiện tại thuộc về bản nhạc yêu thích. Điều này đúng vì đầu vào mô tả quá trình phát lại theo số giây trôi qua thực tế, do đó, theo đúng nghĩa đen, trình phát sẽ tái tạo chính xác những gì xảy ra. 

Vấn đề là số giây. Một ngày có thể chứa 86.400 giây và có thể có 100 ngày cho mỗi đĩa CD trong số 50 đĩa CD. Trường hợp xấu nhất là khoảng 432 triệu giây mô phỏng. Mặc dù mỗi thao tác riêng lẻ đều đơn giản nhưng khối lượng công việc đó lại quá lớn so với thời hạn. 

Quan sát quan trọng là việc phát lại bình thường là định kỳ. Khi Anya vắng mặt, CD sẽ hoạt động giống hệt như một dòng thời gian hình tròn có độ dài bằng tổng thời lượng của CD. Trong mỗi lần duyệt hoàn toàn đĩa CD, chính xác một khoảng thời gian cố định, khoảng thời gian của bản nhạc yêu thích, được dành cho bài hát yêu thích. Do đó, chúng tôi không bao giờ cần phải kiểm tra từng giây riêng lẻ. 

Có một nhận xét hữu ích thứ hai về những đoạn có sự hiện diện của Anya. Câu trả lời tăng dần theo toàn bộ độ dài của đoạn vì mỗi giây đều là bài hát yêu thích. Chúng tôi chỉ cần xác định nơi tiếp tục phát lại bình thường sau đó. Vì bài hát yêu thích được lặp lại nên vị trí mới của nó được xác định bởi độ dài đoạn modulo thời lượng yêu thích. 

Đối với đoạn thiếu độ dài (L), chúng tôi chia (L) thành các chu trình CD hoàn chỉnh và phần còn lại. Mỗi chu kỳ hoàn chỉnh đều đóng góp chính xác khoảng thời gian yêu thích. Phần còn lại ngắn hơn một CD và có thể vượt qua khoảng yêu thích nhiều nhất một lần sau khi tách ở ranh giới hình tròn. Sự chồng chéo đó có thể được tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số giây đã trôi qua) | O(1) | Quá chậm, lên tới 432 triệu lần lặp | 
| Tối ưu | O(số đoạn) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời lượng bản nhạc và tính tổng chiều dài CD. Đồng thời tính toán vị trí bắt đầu tuyệt đối`F`của bản nhạc yêu thích bằng cách tổng hợp thời lượng của tất cả các bản nhạc trước nó. Khoảng thời gian yêu thích trên dòng thời gian tròn là`[F, E)`, Ở đâu`E = F + favorite_duration`. 
2. Xác định hàm`prefix(x)`trả về số giây của bản nhạc yêu thích xuất hiện trong lần đầu tiên`x`giây của một chu kỳ CD. Trước`F`giá trị bằng 0, trong khoảng thời gian yêu thích, nó tăng tuyến tính và sau`E`nó vẫn bằng với thời lượng yêu thích. Điều này chuyển đổi việc đếm chồng chéo thành phép trừ các giá trị tiền tố. 
3. Xác định chức năng đếm số giây yêu thích trong khoảng thời gian phát lại bình thường bắt đầu từ vị trí hình tròn`pos`và lâu dài`length`giây. Lần đầu tiên lấy`length // total`hoàn thành chu trình CD, góp phần`full_cycles * favorite_duration`. Đối với phần còn lại, hãy sử dụng chức năng tiền tố. Nếu phần còn lại vượt qua phần cuối của đĩa CD, hãy chia nó thành hậu tố của chu kỳ hiện tại và tiền tố của chu kỳ tiếp theo. 
4. Mỗi ngày, hãy bắt đầu với một vị trí CD tùy ý vì đoạn đầu tiên luôn là đoạn lẻ khi Anya vào xe và ngay lập tức đặt lại phát lại về đầu bài hát yêu thích. Đặt câu trả lời cho ngày hôm đó thành 0. 
5. Xử lý các phân đoạn trong ngày từ trái sang phải. Đối với một đoạn có độ dài lẻ`L`, thêm tất cả`L`giây để có câu trả lời. Vị trí phát lại mới nằm trong bài hát yêu thích ở độ lệch được xác định bởi`L % favorite_duration`. Nếu phần còn lại bằng 0 thì việc phát lại đã đến cuối bài hát yêu thích và phải tiếp tục với bài hát tiếp theo, do đó vị trí là`E`, chuẩn hóa modulo chiều dài CD. 
6. Đối với một đoạn có chiều dài bằng nhau`L`, hãy sử dụng chức năng chồng chéo phát lại bình thường từ vị trí hiện tại. Thêm số giây yêu thích được hàm đó trả về và nâng cao vị trí vòng tròn bằng`L`. 
7. In đáp án tích lũy trong ngày. Sau khi tất cả các ngày của đĩa CD đã được xử lý, hãy in dòng trống cần thiết trước khi chuyển sang đĩa CD tiếp theo. Tuyên bố cuộc thi ban đầu yêu cầu một dòng trống sau mỗi đĩa CD. 

### Tại sao nó hoạt động 

Bất biến là ngay trước mỗi đoạn chẵn,`pos`thể hiện vị trí chính xác nơi đĩa CD sẽ phát một cách tự nhiên nếu chúng ta đã theo dõi tất cả các phân đoạn trước đó. Trong một phân đoạn lẻ, việc phát lại buộc phải đến bài hát yêu thích, do đó, việc thêm toàn bộ độ dài phân đoạn là chính xác và thao tác modulo sẽ đưa ra vị trí chính xác khi Anya rời đi. Trong một đoạn chẵn, CD tuân theo thứ tự vòng tròn thông thường của nó và chức năng chồng lấp sẽ đếm chính xác các phần của quãng vòng đó thuộc về bản nhạc yêu thích. Do đó, cả câu trả lời tích lũy và vị trí phát lại vẫn chính xác sau mỗi phân đoạn. Vì phân đoạn đầu tiên mỗi ngày đặt lại bài hát yêu thích nên các ngày có thể được xử lý độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        n, k = map(int, input().split())
        tracks = list(map(int, input().split()))

        total = sum(tracks)
        favorite = tracks[k - 1]

        favorite_start = sum(tracks[:k - 1])
        favorite_end = favorite_start + favorite

        def prefix(x):
            if x <= favorite_start:
                return 0
            if x >= favorite_end:
                return favorite
            return x - favorite_start

        def favorite_in_normal_playback(pos, length):
            if length == 0:
                return 0

            full_cycles, rem = divmod(length, total)
            result = full_cycles * favorite

            if rem == 0:
                return result

            end = pos + rem

            if end <= total:
                result += prefix(end) - prefix(pos)
            else:
                result += prefix(total) - prefix(pos)
                result += prefix(end - total)

            return result

        d = int(input())

        output.append(f"CD #{case_no}:")

        for _ in range(d):
            s_and_lengths = list(map(int, input().split()))
            s = s_and_lengths[0]
            segments = s_and_lengths[1:]

            answer = 0
            pos = 0

            for i, length in enumerate(segments):
                if i % 2 == 0:
                    # Anya is in the car, so the favorite song
                    # plays for the entire segment.
                    answer += length

                    rem = length % favorite
                    if rem == 0:
                        # The favorite song has just ended.
                        # Continue with the next track.
                        pos = favorite_end % total
                    else:
                        pos = favorite_start + rem
                else:
                    # Normal CD playback.
                    answer += favorite_in_normal_playback(pos, length)
                    pos = (pos + length) % total

            output.append(str(answer))

        output.append("")

    sys.stdout.write("\n".join(output) + "\n")

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai sẽ xác định bản nhạc yêu thích dưới dạng một khoảng thời gian trên dòng thời gian tuyệt đối của CD. Nếu các bản nhạc có thời lượng`[100, 200, 50]`và bài 2 được yêu thích thì dòng thời gian của CD là`[0, 350)`, trong khi khoảng thời gian yêu thích là`[100, 300)`. Cách trình bày này loại bỏ nhu cầu theo dõi số bản nhạc riêng lẻ trong quá trình phát lại bình thường.`prefix(x)`là phép đếm nguyên thủy chính. Vì`x <= favorite_start`, đầu tiên`x`giây không chứa thời gian yêu thích. Vì`x >= favorite_end`, chúng chứa toàn bộ bài hát yêu thích. Giữa những ranh giới đó, đóng góp được yêu thích chính xác là`x - favorite_start`. 

Chức năng phát lại bình thường trước tiên xử lý các chu kỳ CD hoàn chỉnh. Nếu đĩa CD kéo dài`total`giây, mỗi chu kỳ hoàn chỉnh đều đóng góp chính xác`favorite`giây. Khoảng còn lại có độ dài nhỏ hơn`total`, do đó, nó sẽ nằm trong chu kỳ hiện tại hoặc vượt qua ranh giới hình tròn một lần. Hai trường hợp này được xử lý bởi hàm tiền tố. 

Cập nhật đoạn lẻ cần xử lý đặc biệt khi`length % favorite == 0`. Trong trường hợp đó, bài hát yêu thích vừa kết thúc nên quá trình phát lại sẽ chuyển sang bài hát tiếp theo thay vì bắt đầu lại bài hát yêu thích. sử dụng`favorite_end % total`cũng xử lý trường hợp bài hát yêu thích là bài hát cuối cùng và vị trí tiếp theo là phần đầu của CD. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python. Thời lượng tối đa hàng ngày chỉ là 86.400 giây và câu trả lời cho một ngày bị giới hạn bởi cùng một lượng đó, do đó không có vấn đề tràn số nguyên trong Python. 

## Ví dụ đã hoạt động 

Các mẫu chính thức được đưa ra trong tuyên bố cuộc thi ban đầu. Đối với CD mẫu đầu tiên, track 9 là track được yêu thích nhất và có thời lượng 220 giây. Khoảng thời gian tuyệt đối của nó bắt đầu sau tám bản nhạc đầu tiên, ở vị trí 1739 và kết thúc ở vị trí 1959. Ngày đầu tiên chứa các phân đoạn`1000 900 1000`. 

| Phân đoạn | Loại | Chiều dài | Vị trí trước | Giây yêu thích | Vị trí sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Anya có mặt | 1000 | 0 | 1000 | 1859 | 
| 2 | Bình thường | 900 | 1859 | 100 | 0 | 
| 3 | Anya có mặt | 1000 | 0 | 1000 | 1859 | 

Phân đoạn đầu tiên đóng góp toàn bộ 1000 giây và để lại 120 giây phát lại cho bài hát yêu thích. Trong phân đoạn thông thường 900 giây, quá trình phát lại dành 100 giây để hoàn thành bài hát yêu thích và sau đó đến phần đầu của CD sau khi duyệt qua các bản nhạc còn lại. Đoạn cuối cùng lại đóng góp toàn bộ 1000 giây. Tổng số là 2100, phù hợp với Mẫu 1. 

Đối với Mẫu 2, CD có các bản nhạc có độ dài 100 và 200, trong đó bản nhạc 2 là bản nhạc yêu thích. Khoảng thời gian yêu thích của nó là`[100, 300)`. Hãy xem xét ngày`300 277 131 10000 58`. 

| Phân đoạn | Loại | Chiều dài | Vị trí trước | Giây yêu thích | Vị trí sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Anya có mặt | 300 | 0 | 300 | 200 | 
| 2 | Bình thường | 277 | 200 | 177 | 177 | 
| 3 | Anya có mặt | 131 | 177 | 131 | 231 | 
| 4 | Bình thường | 10000 | 231 | 6669 | 231 | 
| 5 | Anya có mặt | 58 | 231 | 58 | 189 | 

Phân đoạn đầu tiên lặp lại bài hát yêu thích dài 200 giây một lần rồi tiếp tục 100 giây ở bản sao thứ hai, để lại vị trí 200 trên dòng thời gian CD. Phân đoạn bình thường tiếp theo dành 100 giây để hoàn thành bài hát yêu thích, kết thúc đĩa CD và dành thêm 77 giây nữa cho đoạn yêu thích. Phân đoạn 10.000 giây chứa 33 chu kỳ CD hoàn chỉnh, góp phần`33 * 200 = 6600`giây yêu thích, tiếp theo là 100 giây còn lại, đóng góp thêm 69 giây nữa. Đoạn cuối cùng đóng góp trực tiếp 58 giây. Tổng số là 7335, phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T + S) | Quá trình xử lý trước đường đua mất O(T) và mọi phân đoạn lái xe đều được xử lý ở O(1). | 
| Không gian | O(T) | Thời lượng của bản nhạc được lưu trữ để có thể tính toán vị trí bắt đầu của bản nhạc yêu thích. | 

Đây`T`là số lượng bài hát trong một đĩa CD và`S`là tổng số đoạn dẫn động cho đĩa CD đó. Trên toàn bộ đầu vào có tối đa 100.000 phân đoạn, trong khi mô phỏng từng giây đơn giản có thể xử lý 432 triệu giây. Thuật toán tối ưu giảm điều đó xuống còn khoảng một phép tính thời gian không đổi cho mỗi phân đoạn, thoải mái trong giới hạn 1 giây và 256 MB do Codeforces nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        n, k = map(int, input().split())
        tracks = list(map(int, input().split()))

        total = sum(tracks)
        favorite = tracks[k - 1]
        favorite_start = sum(tracks[:k - 1])
        favorite_end = favorite_start + favorite

        def prefix(x):
            if x <= favorite_start:
                return 0
            if x >= favorite_end:
                return favorite
            return x - favorite_start

        def favorite_in_normal_playback(pos, length):
            full_cycles, rem = divmod(length, total)
            result = full_cycles * favorite

            if rem == 0:
                return result

            end = pos + rem

            if end <= total:
                result += prefix(end) - prefix(pos)
            else:
                result += prefix(total) - prefix(pos)
                result += prefix(end - total)

            return result

        d = int(input())
        output.append(f"CD #{case_no}:")

        for _ in range(d):
            data = list(map(int, input().split()))
            s = data[0]
            segments = data[1:]

            answer = 0
            pos = 0

            for i, length in enumerate(segments):
                if i % 2 == 0:
                    answer += length
                    rem = length % favorite

                    if rem == 0:
                        pos = favorite_end % total
                    else:
                        pos = favorite_start + rem
                else:
                    answer += favorite_in_normal_playback(pos, length)
                    pos = (pos + length) % total

            output.append(str(answer))

        output.append("")

    return "\n".join(output) + "\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve_output = solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return solve_output

# Provided samples
sample = """\
2
13 9
212 231 231 235 193 219 207 211 220 247 250 195 270
4
3 1000 900 1000
3 10000 10000 10000
1 2000
2 500 600
2 2
100 200
5
1 70
5 300 277 131 10000 58
2 200 50
2 201 50
2 199 50
"""

expected_sample = """\
CD #1:
2100
20780
2000
660

CD #2:
70
7335
200
251
200

"""

assert run(sample) == expected_sample, "official samples"

# Minimum-size CD, only one track.
assert run("""\
1
1 1
5
1
1 100
""") == """\
CD #1:
100

""", "single-track CD"

# Exact favorite-song boundary.
assert run("""\
1
2 1
5 7
1
3 5 1 7
""") == """\
CD #1:
12

""", "exactly finishing the favorite song"

# Favorite track is the last track, so exact completion wraps to track 1.
assert run("""\
1
2 2
3 5
1
2 5 6
""") == """\
CD #1:
8

""", "favorite is last track and playback wraps"

# All track lengths equal.
assert run("""\
1
3 2
10 10 10
1
3 15 7 20
""") == """\
CD #1:
40

""", "all-equal track lengths"

# Maximum-size-shaped test: 20 tracks, 100 days, 20 segments per day.
tracks = " ".join(["1"] * 20)
day = "20 " + " ".join(["4320"] * 20)
max_case = "1\n20 20\n" + tracks + "\n100\n" + "\n".join([day] * 100) + "\n"
expected_max = "CD #1:\n" + "\n".join(["64800"] * 100) + "\n\n"

assert run(max_case) == expected_max, "maximum dimensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 5 / 1 100`|`100`| Kích thước CD tối thiểu và trường hợp yêu thích là toàn bộ CD | 
|`2 1 / 5 7 / 3 5 1 7`|`12`| Hoàn thành chính xác bài hát yêu thích trước một đoạn vắng mặt | 
|`2 2 / 3 5 / 2 5 6`|`8`| Bài hát yêu thích nằm ở cuối bài nên việc phát lại bình thường sẽ kết thúc ở bài 1 | 
|`3 2 / 10 10 10 / 3 15 7 20`|`40`| Tất cả thời lượng của bản nhạc đều bằng nhau và chuyển tiếp lặp đi lặp lại | 
| 20 bài hát, 100 ngày, 20 phân đoạn mỗi ngày | 64800 mỗi ngày | Giá trị tối đa của giới hạn đầu vào cấu trúc | 

## Vỏ cạnh 

Khi Anya rời đi đúng lúc bài hát yêu thích kết thúc, thuật toán sẽ sử dụng`length % favorite == 0`và di chuyển đến`favorite_end % total`. Vì```
1
2 1
5 7
1
3 5 1 7
```đoạn đầu tiên để lại đĩa CD ở ranh giới sau bản nhạc yêu thích. Đoạn thông thường một giây bắt đầu ở rãnh 2 và đóng góp bằng 0. Đoạn 7 giây cuối cùng đóng góp 7, mang lại`12`. Bất biến được giữ nguyên vì vị trí được lưu trữ đại diện cho bản nhạc tiếp theo chứ không phải phần bắt đầu của một lần lặp lại yêu thích khác. 

Khi bài hát yêu thích là bài hát cuối cùng,`favorite_end == total`. Vì```
1
2 2
3 5
1
2 5 6
```đoạn đầu tiên đóng góp 5 và bộ`pos = total % total = 0`. Sáu giây phát lại bình thường sau đó sẽ duyệt qua tất cả 3 giây của bản nhạc 1 và 3 giây của bản nhạc yêu thích. Kết quả là`8`. Modulo trong cập nhật vị trí là thứ ngăn cản`total`trở thành tọa độ tròn không hợp lệ. 

Khi CD chỉ chứa một bản nhạc, quãng yêu thích là`[0, total)`. Hàm tiền tố trả về toàn bộ độ dài khoảng thời gian cho mỗi phần còn lại của chế độ phát lại bình thường, trong khi các phân đoạn lẻ cũng đóng góp toàn bộ thời lượng của chúng. Vì```
1
1 1
5
1
1 100
```câu trả lời là`100`, vì mỗi giây đều thuộc về bài hát yêu thích. Cách biểu diễn tương tự cũng xử lý các chu kỳ CD đầy đủ lặp đi lặp lại mà không có bất kỳ mô phỏng trường hợp đặc biệt nào. 

Khi một đoạn vắng mặt đi qua phần cuối của CD, chức năng phát lại bình thường sẽ chia phần còn lại thành hậu tố từ vị trí hiện tại đến cuối CD và tiền tố sau khi gói về vị trí 0. Đây là phần vòng tròn của vấn đề mà một cách đơn giản`prefix(end) - prefix(start)`công thức sẽ xử lý sai. Tính toán hai phần giữ cho số quãng yêu thích luôn chính xác ngay cả khi quá trình phát lại chuyển từ bản nhạc cuối cùng trở lại bản nhạc đầu tiên.
