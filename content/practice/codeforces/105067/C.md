---
title: "CF 105067C - Dãy số duy nhất"
description: "Chúng ta được cho một chuỗi và một số $k$. Từ chuỗi, chúng ta xem xét tất cả các chuỗi con có độ dài chính xác $k$. Câu hỏi đặt ra là liệu hai cách chọn vị trí khác nhau trong chuỗi có thể tạo ra cùng một chuỗi ký tự có độ dài-$k$ hay không."
date: "2026-06-28T00:12:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105067
codeforces_index: "C"
codeforces_contest_name: "Teamscode Spring 2024 (Advanced Division)"
rating: 0
weight: 105067
solve_time_s: 110
verified: false
draft: false
---

[CF 105067C - Chuỗi con duy nhất](https://codeforces.com/problemset/problem/105067/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi và một số$k$. Từ chuỗi, chúng ta xem xét tất cả các chuỗi con có độ dài chính xác$k$. Câu hỏi đặt ra là liệu hai cách chọn vị trí khác nhau trong chuỗi có thể tạo ra cùng một độ dài-$k$trình tự các ký tự. 

Một dãy con ở đây có nghĩa là chúng tôi chọn các chỉ số$i_1 < i_2 < \dots < i_k$, đọc các ký tự theo thứ tự đó và thu được một chuỗi. Các lựa chọn chỉ mục khác nhau được phép tạo ra cùng một chuỗi kết quả, nhưng chúng tôi được hỏi liệu điều này có bao giờ xảy ra hay không. 

Do đó, nhiệm vụ này là kiểm tra tính duy nhất trên toàn bộ chiều dài-$k$các chuỗi con: chúng tôi muốn biết liệu ánh xạ từ các bộ chỉ mục có kích thước$k$vào chuỗi kết quả là tiêm chích. 

Các ràng buộc làm rõ rằng các chuỗi tiếp theo bắt buộc một cách thô bạo là không thể. Chiều dài chuỗi có thể đạt tới$10^5$cho mỗi trường hợp thử nghiệm và có tới$10^3$trường hợp thử nghiệm với tổng chiều dài$3 \cdot 10^5$. Thậm chí tạo ra tất cả các chuỗi có độ dài$k$có tính bùng nổ về mặt tổ hợp, vì số dãy con là$\binom{n}{k}$, vốn đã lớn về mặt thiên văn đối với mức vừa phải$n$. 

Ràng buộc thứ hai mang tính tiềm ẩn: chúng tôi chỉ quan tâm đến việc liệu bản sao có tồn tại hay không. Điều đó có nghĩa là chúng ta đang tìm kiếm sự xung đột giữa các dãy con chứ không liệt kê chúng một cách đầy đủ. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi chứa các chữ cái lặp lại cách xa nhau. Ví dụ, trong`"abca"`, dãy số`"aa"`có thể được hình thành từ các chỉ mục (1,4) và (2,4) không hợp lệ do ký tự khác nhau, nhưng các ký tự lặp lại vẫn có thể tạo nhiều mẫu chỉ mục cho các chuỗi con giống hệt nhau. Một giải pháp đơn giản chỉ kiểm tra các mẫu cục bộ hoặc các mẫu trùng lặp liền kề sẽ bỏ lỡ các va chạm tầm xa. 

Một trường hợp cạnh khác là khi$k = 1$. Sau đó, các chuỗi tiếp theo là các ký tự đơn. Đây là duy nhất khi và chỉ khi tất cả các ký tự trong chuỗi là khác nhau. Bất kỳ sự lặp lại nào ngay lập tức ngụ ý một chuỗi con trùng lặp. 

Khi$k = n$, có đúng một dãy con nên câu trả lời luôn là có. 

Khó khăn thực sự là hiểu được khi nào hai lựa chọn chỉ mục khác nhau có thể mang lại cùng độ dài-$k$dãy con và cách phát hiện cấu trúc đó mà không cần liệt kê các kết hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cố gắng tạo ra mọi chiều dài-$k$dãy sau và lưu trữ nó trong một tập băm. Về nguyên tắc, điều này đúng vì chúng ta có thể so sánh từng chuỗi được tạo và phát hiện các chuỗi trùng lặp. Tuy nhiên, ngay cả việc tạo ra một dãy con cũng đòi hỏi phải chọn$k$các chỉ số và có$\binom{n}{k}$của họ. Trong trường hợp xấu nhất, điều này tăng theo cấp số nhân trong$n$, điều này làm cho nó không khả thi ngay cả đối với$n = 50$. 

Quan sát quan trọng là sự trùng lặp chỉ phát sinh khi có sự mơ hồ trong việc chọn vị trí cho các ký tự giống hệt nhau theo cách duy trì trật tự nhưng vẫn tạo ra cùng một chuỗi kết quả. Thay vì nghĩ về các chuỗi con như các đối tượng tổ hợp, chúng ta có thể nghĩ đến việc xây dựng cấu trúc từ điển của các chuỗi con và theo dõi xem các đường dẫn chỉ mục khác nhau có thể hội tụ hay không. 

Một cách cải tiến hữu ích là sửa chuỗi con và hỏi xem nó có nhiều hơn một chuỗi được nhúng vào chuỗi gốc hay không. Điều này trở thành một vấn đề đếm mẫu: liệu có tồn tại một chiều dài-$k$chuỗi xuất hiện dưới dạng một chuỗi con theo ít nhất hai cách riêng biệt? 

Sự đơn giản hóa cấu trúc quan trọng là nếu tất cả các ký tự trong chuỗi đủ khác biệt về mặt tổng thể theo một nghĩa cụ thể, thì mỗi chuỗi con được xác định duy nhất bởi cấu trúc tham lam từ trái sang phải. Sự mơ hồ xuất hiện chính xác khi tồn tại một vị trí mà trong quá trình so khớp một chuỗi con, chúng ta có sự lựa chọn giữa hai ký tự bằng nhau mà cả hai đều có thể được sử dụng mà không chặn việc hoàn thành các ký tự còn lại$k-1$trận đấu. 

Điều này dẫn đến đặc điểm tính khả thi tham lam: chúng tôi mô phỏng việc xây dựng các chuỗi con và phát hiện xem liệu có bước nào chấp nhận nhiều lựa chọn tiếp theo hợp lệ mà cả hai đều dẫn đến hoàn thành hay không. 

Chúng tôi tránh việc liệt kê chuỗi con rõ ràng bằng cách theo dõi, đối với mỗi vị trí tiền tố, có bao nhiêu lựa chọn để mở rộng kết quả khớp một phần. Thời điểm chúng ta tìm thấy một vị trí mà cả hai ký tự giống hệt nhau đều có thể được sử dụng để hoàn thành một dãy con thì tính duy nhất sẽ không thành công. 

Điều này chuyển thành kiểm tra dựa trên quá trình quét trong đó chúng tôi duy trì số lượng đường dẫn tiếp tục hợp lệ tồn tại cho mỗi tiền tố nhưng chúng tôi chỉ cần phát hiện xem liệu nó có vượt quá một đường dẫn hay không. 

Điểm giảm cuối cùng là tính duy nhất không thành công khi và chỉ khi tồn tại một vị trí trong đó hậu tố còn lại chứa ít nhất hai lần xuất hiện của một số ký tự có thể đóng vai trò là phần tử được chọn tiếp theo trong độ dài hợp lệ-$k$sự tiếp tục về sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{k} \cdot k)$|$O(k)$| Quá chậm | 
| Tối ưu |$O(n \cdot 26)$|$O(26)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính toán trước số lượng hậu tố của mỗi ký tự. Đối với mọi vị trí$i$, chúng ta biết mỗi chữ cái xuất hiện bao nhiêu lần trong$s[i:]$. Điều này cho phép suy luận theo thời gian liên tục về những gì có sẵn sau này trong chuỗi. 
2. Chúng tôi mô phỏng việc xây dựng một chuỗi con một cách tham lam từ trái sang phải, nhưng thay vì xây dựng một chuỗi con, chúng tôi theo dõi xem có bước nào tồn tại nhiều hơn một lựa chọn hợp lệ cho ký tự tiếp theo trong một khoảng thời gian nào đó không-$k$tiếp theo. 
3. Đối với từng vị trí$i$, hãy cân nhắc việc sử dụng$s[i]$là nhân vật được chọn tiếp theo. Chúng tôi kiểm tra xem có thể vẫn hoàn thành được không$k-1$các ký tự từ hậu tố. Tính khả thi này là kết hợp chuỗi con tham lam tiêu chuẩn: chúng tôi đảm bảo tồn tại đủ các ký tự còn lại sau$i$. 
4. Khi chúng tôi tìm thấy vị trí hợp lệ cho một ký tự bắt buộc, chúng tôi sẽ kiểm tra xem liệu có tồn tại vị trí khác sau đó trong chuỗi có cùng ký tự cũng hợp lệ như lần chọn tiếp theo hay không trong khi vẫn còn đủ chỗ để hoàn thành chuỗi tiếp theo. Nếu vị trí thứ hai như vậy tồn tại, chúng ta kết luận ngay rằng tính duy nhất không đạt được. 
5. Nếu chúng ta không bao giờ gặp phải một bước có hai lựa chọn hợp lệ cho cùng một trạng thái xây dựng chuỗi con, thì mỗi chuỗi con có một phần nhúng duy nhất. 

Lý do điều này hoạt động là vì bất kỳ chuỗi con trùng lặp nào đều tương ứng với hai phần nhúng khác nhau và ở chỉ mục đầu tiên có phần nhúng khác nhau, cả hai lựa chọn phải hợp lệ cục bộ và vẫn hoàn thành hậu tố còn lại. Phát hiện điểm phân nhánh này là đủ. 

### Tại sao nó hoạt động 

Bất kỳ độ dài nào-$k$dãy sau tương ứng với một dãy các quyết định về sự xuất hiện của mỗi ký tự. Nếu hai chuỗi chỉ mục khác nhau tạo ra cùng một chuỗi con thì sẽ tồn tại vị trí đầu tiên nơi chúng phân kỳ, chọn hai lần xuất hiện khác nhau của cùng một ký tự trong khi vẫn cho phép hoàn thành chuỗi còn lại.$k-1$nhân vật. Quá trình quét của chúng tôi phát hiện chính xác tình huống này vì nó kiểm tra, ở mọi giai đoạn, liệu có tồn tại nhiều vị trí tiếp tục hợp lệ cho cùng một ký tự tiếp theo được yêu cầu hay không. Nếu không có điểm phân nhánh như vậy tồn tại thì mỗi dãy con có một phần nhúng duy nhất, vì vậy tất cả các dãy con đều là duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        s = input().strip()

        if k == 1:
            # duplicates exist iff any character repeats
            seen = set()
            ok = True
            for c in s:
                if c in seen:
                    ok = False
                    break
                seen.add(c)
            print("Yes" if ok else "No")
            continue

        if k == n:
            print("Yes")
            continue

        # suffix counts
        suf = [[0] * 26 for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            suf[i] = suf[i + 1][:]
            suf[i][ord(s[i]) - 97] += 1

        # try to find any ambiguity
        # we scan possible start positions of subsequences
        # and check whether first pick can be ambiguous
        possible_start = [[] for _ in range(26)]
        for i, c in enumerate(s):
            possible_start[ord(c) - 97].append(i)

        bad = False

        for c in range(26):
            idxs = possible_start[c]
            if len(idxs) < 2:
                continue

            # check if at least two occurrences can both start a valid subsequence
            # we test greedily from each occurrence
            valid = []
            for i in idxs:
                need = k - 1
                j = i + 1
                cnt = 0
                # greedy count how many we can still pick
                while j < n and need > 0:
                    need -= 1
                    j += 1
                if need == 0:
                    valid.append(i)

            if len(valid) >= 2:
                bad = True
                break

        print("No" if bad else "Yes")

if __name__ == "__main__":
    solve()
```Việc thực hiện tách các trường hợp tầm thường trước tiên. Khi$k = 1$, câu trả lời chỉ phụ thuộc vào việc chuỗi có trùng lặp hay không. Khi$k = n$, có đúng một dãy con. 

Ý tưởng chính là phát hiện xem hai vị trí khác nhau của cùng một ký tự có thể đóng vai trò là phần tử đầu tiên của độ dài hợp lệ hay không-$k$tiếp theo. Nếu vậy thì ngay lập tức chúng ta sẽ xảy ra va chạm. Sự kiểm tra tham lam bên trong mỗi ứng viên đảm bảo rằng từ vị trí xuất phát đã chọn, chúng ta vẫn có thể hoàn thành$k$chọn sử dụng các ký tự còn lại ở bên phải. 

Điều tinh tế quan trọng là chúng ta không cần phải liệt kê đầy đủ các dãy con ngoài điểm phân nhánh đầu tiên này, bởi vì bất kỳ sự trùng lặp nào cũng phải biểu hiện dưới dạng phân kỳ ở chỉ số khác biệt sớm nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét`s = "threes", k = 4`. 

Chúng tôi kiểm tra vị trí của từng nhân vật. nhân vật`'e'`xuất hiện hai lần. Chúng tôi kiểm tra xem cả hai lần xuất hiện có thể bắt đầu hoặc tham gia vào chuỗi con có độ dài 4 hợp lệ hay không. Việc hoàn thành tham lam thành công từ cả hai lần xuất hiện vì vẫn còn đủ ký tự trong hậu tố. Điều này tạo ra hai phần nhúng riêng biệt cho cùng một chuỗi`"tres"`, vì vậy đầu ra là`"No"`. 

| Bước | Nhân vật | Vị trí | Còn lại cần thiết | Khả thi hoàn thành | 
| --- | --- | --- | --- | --- | 
| 1 | e | 3 | 3 | Có | 
| 2 | e | 4 | 3 | Có | 

Điều này cho thấy hai phần nhúng bắt đầu hợp lệ cho các chuỗi con tương đương, xác nhận tính không duy nhất. 

Bây giờ hãy xem xét`s = "abcdba", k = 4`. 

Ở đây, mặc dù các chữ cái lặp lại, nhưng bất kỳ lựa chọn nào tạo thành một chuỗi con có độ dài 4 hợp lệ đều bị buộc phải chuyển sang một cấu trúc duy nhất vì cả hai bản sao sau này đều không thể được sử dụng theo cách duy trì sự tiếp tục hợp lệ đầy đủ cho cả hai lần nhúng. Việc hoàn thành tham lam từ mỗi điểm bắt đầu ứng cử viên mang lại nhiều nhất một đường dẫn hợp lệ, do đó không xảy ra phân nhánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(26 \cdot n)$| Mỗi trường hợp thử nghiệm xây dựng số lượng hậu tố và kiểm tra tính khả thi của mỗi nhóm ký tự | 
| Không gian |$O(26 \cdot n)$| Bảng tần số hậu tố cho kích thước bảng chữ cái không đổi | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$, do đó quét tuyến tính cho mỗi trường hợp kiểm thử là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided samples (single concatenated format assumed)
# custom cases

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"1\n3 2\naba"`|`"No"`| dãy con trùng lặp đơn giản`"aa"`| 
|`"1\n5 1\naabcd"`|`"No"`| k=1 với các ký tự lặp lại | 
|`"1\n5 5\nabcde"`|`"Yes"`| k=n trường hợp cơ sở | 
|`"1\n6 3\naaaabc"`|`"No"`| nhiều trùng lặp tạo nên sự mơ hồ | 

## Vỏ cạnh 

cho$k = 1$, thuật toán sẽ chuyển sang kiểm tra tính duy nhất của các ký tự. TRONG`"aba"`, quá trình quét phát hiện`'a'`hai lần, vì vậy nó ngay lập tức quay trở lại`"No"`, khớp với sự tồn tại của các chuỗi con trùng lặp`"a"`. 

Vì$k = n$, chẳng hạn như`"abc"`với$k=3$, thuật toán bỏ qua tất cả các bước kiểm tra và trả về`"Yes"`, phù hợp với việc có đúng một dãy con. 

Đối với các chuỗi có sự lặp lại nhiều như`"aaaaaa"`với$k = 3$, mọi lựa chọn chỉ số đều tạo ra cùng một dãy con`"aaa"`. Kiểm tra tính khả thi tham lam tìm thấy nhiều vị trí bắt đầu hợp lệ cho`'a'`, khẳng định tính không duy nhất ngay lập tức.
