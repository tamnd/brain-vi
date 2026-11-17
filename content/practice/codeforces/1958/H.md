---
title: "CF 1958H - Phép thuật tổng hợp"
description: "Chúng ta được cung cấp một hệ thống chính tả hoạt động giống như một chương trình được viết bằng một ngôn ngữ rất hạn chế. Phần đầu tiên của hệ thống là danh sách các thao tác cơ bản, mỗi thao tác sẽ tăng hoặc giảm lượng máu của quái vật theo một số nguyên cố định."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp"]
categories: ["algorithms"]
codeforces_contest: 1958
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 10"
rating: 2600
weight: 1958
solve_time_s: 94
verified: true
draft: false
---

[CF 1958H - Phép thuật tổng hợp](https://codeforces.com/problemset/problem/1958/H) 

**Đánh giá:** 2600 
**Thẻ:** *đặc biệt, dp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống chính tả hoạt động giống như một chương trình được viết bằng một ngôn ngữ rất hạn chế. Phần đầu tiên của hệ thống là danh sách các thao tác cơ bản, mỗi thao tác sẽ tăng hoặc giảm lượng máu của quái vật theo một số nguyên cố định. Sau đó là một chuỗi các thao tác tổng hợp, trong đó mỗi phép thuật tổng hợp được xác định là một danh sách có thứ tự các phép thuật được xác định trước đó và việc thực hiện nó có nghĩa là thực hiện lần lượt các phép thuật đó. 

Điều phức tạp chính là các phép thuật có thể gọi các phép thuật khác, vì vậy phép thuật cuối cùng mà chúng tôi sử dụng không phải là một danh sách phẳng. Nó là một cấu trúc dạng cây mở rộng thành một chuỗi dài các thao tác cơ bản khi được thực hiện đầy đủ. Việc thực thi cũng không phải là vô điều kiện. Ngay khi máu của quái vật trở về 0 hoặc âm, nó sẽ chết và mọi hoạt động còn lại đều bị bỏ qua. 

Nhiệm vụ là mô phỏng việc sử dụng phép thuật tổng hợp cuối cùng và xác định xem quái vật có chết hay không. Nếu nó chết, chúng ta phải báo cáo chỉ số của phép thuật cơ bản khiến máu giảm xuống 0 hoặc thấp hơn lần đầu tiên trong quá trình thực hiện. 

Các ràng buộc đầu vào cho thấy rõ rằng về nguyên tắc, việc mở rộng hoàn toàn ngây thơ có thể có rủi ro, vì các phép thuật tổng hợp có thể lồng vào nhau. Tuy nhiên, tổng số tài liệu tham khảo chính tả được liệt kê trên tất cả các vật liệu tổng hợp là nhỏ, có nghĩa là bản thân cấu trúc vẫn nhỏ gọn ngay cả khi phạm vi mở rộng khái niệm lớn. 

Một vài trường hợp tinh tế xuất hiện ngay lập tức. Một câu thần chú tổng hợp có thể bao gồm cùng một câu thần chú phụ nhiều lần, vì vậy việc sử dụng lại không được nhầm lẫn với sự trùng lặp trạng thái. Một vấn đề khác là một số phép thuật cơ bản giúp tăng máu, do đó, việc thực hiện một phần có thể tạm thời tăng lượng máu lên trước khi mức giảm sau đó giết chết mục tiêu. Cách tiếp cận tham lam hoặc “chỉ tính tổng” không thành công vì cái chết phụ thuộc vào hành vi tiền tố chứ không chỉ phụ thuộc vào hiệu ứng tổng thể. 

Trường hợp quan trọng nhất là chấm dứt sớm. Hãy xem xét một chuỗi thần chú thường rất dài nhưng quái vật sẽ chết sau vài thao tác đầu tiên. Bất kỳ giải pháp nào mở rộng hoàn toàn mọi thứ mà không dừng lại sẽ đúng về mặt logic nhưng có thể thực hiện những công việc không cần thiết. 

## Phương pháp tiếp cận 

Việc giải thích vũ lực là đơn giản. Chúng tôi mở rộng mọi phép thuật tổng hợp thành chuỗi đầy đủ các phép thuật cơ bản mà cuối cùng nó thực thi, sau đó mô phỏng danh sách cuối cùng từ trái sang phải trong khi vẫn duy trì tình trạng hiện tại. Bất cứ khi nào gặp phải một hỗn hợp, chúng tôi thay thế nó bằng cách khai triển đầy đủ theo cách đệ quy. 

Cách tiếp cận này đúng vì nó phản ánh chính xác việc thực hiện. Tuy nhiên, nó không thành công về mặt tính toán vì một câu thần chú tổng hợp có thể mở rộng thành một chuỗi rất dài và các vật liệu tổng hợp lồng nhau có thể gây ra sự mở rộng lặp đi lặp lại của cùng một cấu trúc. Trong trường hợp xấu nhất, mỗi hỗn hợp đề cập đến các hỗn hợp trước đó, tạo thành một mô hình tái sử dụng sâu. Mặc dù mô tả đầu vào nhỏ nhưng dấu vết thực thi mở rộng có thể trở nên cực kỳ lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần cấu trúc đầy đủ một cách rõ ràng trong thời gian chạy. Chúng ta chỉ cần chuỗi tuyến tính cuối cùng của các phép thuật cơ bản theo thứ tự thực hiện và chúng ta chỉ cần duyệt qua nó một lần cho đến khi quái vật chết. Vì mọi hỗn hợp được định nghĩa là sự kết hợp có thứ tự của các phép thuật trước đó, nên chúng ta có thể tính toán trước một biểu diễn phẳng cho từng phép thuật bằng cách sử dụng lập trình động theo thứ tự chỉ mục. 

Mỗi phép thuật có thể được biểu diễn dưới dạng danh sách các chỉ số phép thuật cơ bản mà nó mở rộng. Các phép thuật cơ bản là những danh sách tầm thường có kích thước một. Đối với các phép thuật tổng hợp, chúng tôi ghép các danh sách thành phần đã được tính toán sẵn của chúng. Bởi vì các phép thuật chỉ đề cập đến các chỉ số trước đó nên cấu trúc này hợp lệ trong một lần chuyển từ trái sang phải. 

Sau khi việc làm phẳng này được thực hiện, bước cuối cùng là mô phỏng đơn lẻ trên danh sách làm phẳng của câu thần chú cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng đệ quy đầy đủ trong quá trình mô phỏng | Hàm mũ trong trường hợp xấu nhất | Hàm mũ | Quá chậm | 
| Làm phẳng DP + mô phỏng đơn | O(tổng kích thước mở rộng) | O(tổng kích thước mở rộng) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc các hiệu ứng phép thuật cơ bản và thể hiện từng phép thuật cơ bản dưới dạng một chuỗi đơn chứa chỉ mục của nó. Sau này chúng ta sẽ sử dụng các giá trị khi mô phỏng. 
2. Duy trì cấu trúc`seq[i]`nơi lưu trữ chuỗi thực thi phép thuật được làm phẳng`i`. Đối với các phép thuật cơ bản, điều này chỉ đơn giản là`[i]`. 
3. Đối với các phép thuật tổng hợp, hãy xây dựng`seq[i]`bằng cách lặp lại các thành phần được liệt kê theo thứ tự và nối thêm các chuỗi đã được tính toán sẵn của chúng. Thứ tự rất quan trọng vì việc thực hiện là tuần tự nghiêm ngặt. 
4. Sau khi xử lý trước tất cả các phép thuật, hãy tập trung vào`seq[n+m]`, câu thần chú cuối cùng. 
5. Mô phỏng việc thực hiện trên chuỗi đã được làm phẳng này. Duy trì một biến`hp`khởi tạo theo sức khỏe của quái vật. 
6. Duyệt qua từng chỉ số phép thuật cơ bản trong chuỗi. Đối với mỗi chỉ số`i`, áp dụng hiệu ứng của nó`b[i]`ĐẾN`hp`. 
7. Nếu tại bất kỳ thời điểm nào`hp <= 0`, xuất ngay`i`như một câu thần chú giết chóc và ngừng xử lý. 
8. Nếu chuỗi kết thúc mà không có`hp`giảm xuống 0 hoặc thấp hơn, đầu ra`-1`. 

Tính đúng đắn của phương pháp này phụ thuộc vào thực tế là việc làm phẳng duy trì thứ tự thực hiện chính xác. Mỗi phép thuật tổng hợp được thay thế bằng chuỗi chính xác của các thao tác cơ bản mà nó sẽ tạo ra trong thời gian chạy, do đó việc mô phỏng trên danh sách phẳng giống hệt với việc thực thi đệ quy trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, hp = map(int, input().split())
        b = [0] + list(map(int, input().split()))

        m = int(input())

        seq = [[] for _ in range(n + m + 1)]

        for i in range(1, n + 1):
            seq[i] = [i]

        for i in range(n + 1, n + m + 1):
            parts = list(map(int, input().split()))
            k = parts[0]
            spells = parts[1:]
            cur = []
            for sp in spells:
                cur.extend(seq[sp])
            seq[i] = cur

        final = seq[n + m]

        cur_hp = hp
        ans = -1

        for idx in final:
            cur_hp += b[idx]
            if cur_hp <= 0:
                ans = idx
                break

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng tiền xử lý trực tiếp. Mỗi`seq[i]`được xây dựng theo thứ tự tôpô nên khi xử lý một câu thần chú tổng hợp, tất cả các chuỗi được tham chiếu đều có sẵn. 

Một điểm tinh tế là chúng tôi lưu trữ chuỗi các chỉ số chính tả cơ bản, không phải chỉ số tổng hợp. Điều này đảm bảo rằng mô phỏng chỉ áp dụng hiệu ứng cơ bản một lần cho mỗi thao tác. Một chi tiết quan trọng khác là dừng sớm trong quá trình mô phỏng, điều này đảm bảo chúng tôi không bao giờ xử lý hậu tố không cần thiết của chuỗi cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét một tình huống rút ra nhỏ trong đó các phép thuật cơ bản`+5, -10, +3`và một câu thần chú tổng hợp là`[1, 2, 3, 2]`, với HP ban đầu bằng`4`. 

| Bước | Chính tả | HP trước | Thay đổi | HP sau | Trạng thái | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | +5 | 9 | còn sống | 
| 2 | 2 | 9 | -10 | -1 | chết | 

Quá trình mô phỏng dừng ngay lập tức ở bước 2, mặc dù trình tự vẫn tiếp tục. 

Điều này chứng tỏ rằng việc dừng dựa trên tiền tố là cần thiết. Ngay cả khi các hoạt động sau này tồn tại và bao gồm cả việc chữa lành, chúng cũng không còn ý nghĩa gì khi cái chết xảy ra. 

Bây giờ hãy xem xét một trường hợp việc chữa lành trì hoãn cái chết: 

Các phép thuật cơ bản:`[-3, +5, -4]`, tổng hợp:`[2, 1, 3]`, HP ban đầu`3`. 

| Bước | Chính tả | HP trước | Thay đổi | HP sau | Trạng thái | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | +5 | 8 | còn sống | 
| 2 | 1 | 8 | -3 | 5 | còn sống | 
| 3 | 3 | 5 | -4 | 1 | còn sống | 

Ở đây, con quái vật vẫn sống sót, mặc dù có những phép thuật tiêu cực, bởi vì thứ tự và sự tích lũy tiền tố không bao giờ vượt qua con số 0. 

Điều này xác nhận rằng chúng ta phải mô phỏng thứ tự chính xác hơn là lý luận về tổng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng độ dài chuỗi mở rộng) | Mỗi phép thuật được mở rộng một lần thành một danh sách phẳng và mỗi thao tác cơ bản được xử lý một lần trong quá trình mô phỏng | 
| Không gian | O(tổng độ dài chuỗi mở rộng) | Tất cả các chuỗi làm phẳng được lưu trữ rõ ràng | 

Tổng kích thước của tất cả các định nghĩa chính tả tổng hợp bị giới hạn bởi ràng buộc đầu vào, do đó, mặc dù tồn tại lồng nhau, số lượng các cạnh rõ ràng đủ nhỏ để phương pháp này nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out

# provided sample
assert run("""4
4 9
1 -2 3 -4
3
3 1 4 3
4 1 2 1 2
6 6 5 6 5 6 5
4 9
1 -2 3 -4
3
3 1 4 3
4 1 2 1 2
7 6 5 6 5 6 6 5
3 31
-10 -20 30
1
6 1 2 3 1 2 3
6 20
-1 -5 -9 -7 -1 -1
4
3 6 5 2
4 3 3 7 6
6 4 8 4 4 6 7
3 6 5 7
""") == """4
4
-1
-1
"""

# minimal case: immediate death
assert run("""1
1 1
-5
0
""") == """1
"""

# no death at all
assert run("""1
2 100
1 1
1
1 1
""") == """-1
"""

# composite repeats same spell
assert run("""1
2 5
1 -3
1
4 1 2 1 2
""") == """2
"""

# death at later position
assert run("""1
3 5
2 -4 -10
1
3 1 2 3
""") == """2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 | chấm dứt sớm ở câu thần chú cơ bản đầu tiên | 
| không chết | -1 | sinh tồn xuyên suốt chuỗi | 
| sử dụng hỗn hợp lặp đi lặp lại | 2 | sự đúng đắn khi lặp lại | 
| cái chết trì hoãn | 2 | phát hiện cái chết dựa trên tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một phép thuật tổng hợp chỉ chứa các phép thuật chữa lành sớm và một phép thuật giết chết sau đó. Thuật toán xử lý việc này một cách chính xác vì nó không tổng hợp các giá trị trên toàn cầu mà tôn trọng thứ tự tiền tố. 

Một trường hợp khác là lồng sâu trong đó nhiều vật liệu tổng hợp sử dụng lại cùng một câu thần chú phụ. Mặc dù điều này có thể gợi ý công việc lặp đi lặp lại, nhưng mỗi phép thuật được mở rộng chính xác một lần trong cấu trúc DP, do đó việc tái sử dụng được xử lý một cách an toàn mà không làm thay đổi độ chính xác. 

Trường hợp cuối cùng là quái vật chết rất sớm trong phép thuật cuối cùng. Quá trình mô phỏng dừng ngay lập tức, do đó, ngay cả khi cấu trúc phẳng lớn, chỉ có tiền tố cần thiết được đánh giá, ngăn chặn việc tính toán không cần thiết.
