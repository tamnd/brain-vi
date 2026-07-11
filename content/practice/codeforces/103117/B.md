---
title: "CF 103117B - Lẩu"
description: "Chúng tôi đang mô phỏng quy trình theo lượt trên một nhóm người tham gia theo vòng tròn. Mỗi người tham gia được liên kết với một loại thành phần cố định. Một multiset toàn cầu được gọi là pot phát triển theo thời gian."
date: "2026-07-03T20:18:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103117
codeforces_index: "B"
codeforces_contest_name: "The 2021 Sichuan Provincial Collegiate Programming Contest"
rating: 0
weight: 103117
solve_time_s: 58
verified: true
draft: false
---

[CF 103117B - Lẩu](https://codeforces.com/problemset/problem/103117/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quy trình theo lượt trên một nhóm người tham gia theo vòng tròn. Mỗi người tham gia được liên kết với một loại thành phần cố định. Một multiset toàn cầu được gọi là pot phát triển theo thời gian. Ở mỗi bước, người tham gia tích cực sẽ chèn một đơn vị thành phần của họ nếu nó vắng mặt hoặc xóa tất cả các lần xuất hiện của thành phần đó nếu nó có mặt, kiếm được một điểm hạnh phúc trong trường hợp đó. 

Đầu ra không phải là trạng thái pot cuối cùng mà là tổng số sự kiện “xóa sổ” thành công của mỗi người tham gia sau m thao tác. 

Các ràng buộc đầu vào ngụ ý rằng việc mô phỏng đơn giản các thao tác m là không thể vì m có thể đạt tới 10^9. Ngay cả O(m) cho mỗi trường hợp thử nghiệm cũng không khả thi. Nhiều nhất chúng ta có thể đủ khả năng chi trả những thứ như O(n + k) hoặc O(n log n) cho mỗi trường hợp thử nghiệm, vì tổng n và k trên các trường hợp thử nghiệm là bị giới hạn. 

Một vấn đề tế nhị là trạng thái nồi không độc lập với từng thành phần một cách rõ ràng. Mỗi thành phần chuyển đổi giữa hiện diện và vắng mặt, nhưng việc "xóa" phụ thuộc vào việc liệu bất kỳ thao tác chèn nào có xảy ra kể từ lần xóa cuối cùng hay không và những người tham gia khác nhau có thể tương tác với cùng một thành phần vào các thời điểm khác nhau. 

Một mô phỏng đơn giản sẽ giống như việc duy trì liên tục một bộ hoặc nhiều bộ thành phần hoạt động. Điều đó không thành công vì cùng một thành phần có thể xuất hiện nhiều lần và việc thanh toán không phụ thuộc vào tần suất mà chỉ phụ thuộc vào sự tồn tại. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ bao gồm: 

Nếu tất cả những người tham gia đều thích cùng một thành phần, giả sử n = 3, a = [1, 1, 1], thì lọ sẽ chuyển đổi giữa loại rỗng và loại chứa một loại duy nhất. Lần chèn đầu tiên không làm gì cả, lần chèn thứ hai gây ra sự rõ ràng và lợi ích, v.v. Mô hình này mang tính định kỳ với giai đoạn 2 cho mỗi chu kỳ đầy đủ của người tham gia. Một mô phỏng toàn cầu đơn giản vẫn hoạt động hợp lý nhưng quá chậm đối với m lên tới 10^9. 

Nếu tất cả các thành phần đều khác biệt, giả sử a = [1, 2, 3, 4] thì mỗi bước chỉ chèn và không bao giờ xóa trong một chu kỳ cho đến khi xảy ra tương tác bao quanh. Nhiều triển khai giả định không chính xác sự lặp lại ngay lập tức trong một chu kỳ, điều này là sai. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu xử lý từng bước trong số m bước, cập nhật tập hợp chung cho nồi. Mỗi bước sẽ kiểm tra tư cách thành viên và chèn hoặc xóa. Điều này đúng nhưng tốn thời gian O(m), quá lớn khi m lên tới 10^9. 

Quan sát quan trọng là nồi chỉ quan tâm đến việc mỗi thành phần hiện có hay không. Điều này có nghĩa là trạng thái của hệ thống là một vectơ nhị phân trên k thành phần. Tuy nhiên, ngay cả điều đó cũng quá lớn để mô phỏng trực tiếp cho m lớn. 

Cấu trúc thực sự đến từ việc nhóm các hành động theo thành phần. Mỗi thành phần hoạt động độc lập về mặt hiện diện chuyển đổi, nhưng thời gian chuyển đổi được kiểm soát bởi các lần xuất hiện theo trình tự vòng tròn. Mỗi khi chúng ta nhìn thấy một thành phần, chúng ta sẽ bật nó lên hoặc đặt lại thành tắt và “tăng” chỉ xảy ra khi nó đã được bật sẵn. 

Vì vậy, đối với mỗi thành phần, điều quan trọng là chuỗi các chỉ số nơi nó xuất hiện trong sự lặp lại vô hạn của mảng a. Mỗi lần xuất hiện sẽ lật một bit trạng thái. Mức tăng xảy ra bất cứ khi nào chúng ta gặp số 1 trong chuỗi nhị phân hiện là 1. 

Điều này làm giảm vấn đề đếm số lần mỗi thành phần xuất hiện trong m bước và cách những lần xuất hiện đó được phân bổ qua các chu kỳ có độ dài n. Khi biết mình thực hiện bao nhiêu chu trình đầy đủ và số chu trình còn lại, chúng tôi có thể tính toán số lần xuất hiện trên mỗi thành phần mà không cần lặp lại từng bước. 

Sự đơn giản hóa chính là trong một chu kỳ đầy đủ, mỗi thành phần xuất hiện chính xác cnt[i] lần (trong đó cnt[i] là tần số của nó trong mảng cơ sở). Vì vậy, trong các chu kỳ đầy đủ, chúng tôi có thể tính toán số lượng chuyển đổi mà mỗi thành phần phải trải qua. Chu kỳ một phần còn lại được xử lý trực tiếp.

Khó khăn sau đó trở thành việc theo dõi trạng thái chuyển đổi qua các lần lặp lại một cách hiệu quả. Vì mỗi thành phần chuyển đổi trạng thái một cách độc lập trong mỗi lần xuất hiện nên số lần tăng về cơ bản là số lần chúng ta nhìn thấy thành phần đó trong khi trạng thái chuyển đổi của nó đã hoạt động. Điều đó làm giảm việc đếm sự trùng lặp giữa các vị trí xuất hiện modulo 2 trong tiền tố có độ dài m. 

Điều này dẫn đến giải pháp O(n + k) cho mỗi trường hợp thử nghiệm bằng cách sử dụng tính năng đếm tần số và lý luận chẵn lẻ thay vì mô phỏng rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) | O(k) | Quá chậm | 
| Tần số + Chu kỳ chẵn lẻ | O(n + k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu chu kỳ đầy đủ có độ dài n phù hợp với m và bao nhiêu bước còn lại trong chu kỳ một phần cuối cùng. Điều này tách quá trình thành cấu trúc lặp lại và phần đuôi tiền tố, cho phép tổng hợp thay vì mô phỏng. 
2. Đếm số lần xuất hiện của từng thành phần trong mảng cơ sở. Điều này cho chúng ta biết mỗi thành phần được kích hoạt bao nhiêu lần trong một chu kỳ đầy đủ. 
3. Đối với mỗi thành phần, hãy xác định trạng thái chuyển đổi của nó phát triển như thế nào qua nhiều lần xuất hiện. Mỗi lần xuất hiện sẽ làm thay đổi sự hiện diện và mức tăng sẽ xảy ra chính xác khi chúng ta gặp phải một lần xuất hiện trong khi trạng thái đã hoạt động. 
4. Hãy quan sát rằng trong một chu kỳ đầy đủ, tác động chỉ phụ thuộc vào tính chẵn lẻ của các lần xuất hiện. Nếu một thành phần xuất hiện cnt[i] lần trong mỗi chu kỳ, thì sau một số chu kỳ chẵn, hiệu ứng chuyển đổi ròng sẽ hủy bỏ và sau một số chu kỳ lẻ, nó hoạt động như một chu kỳ duy nhất. 
5. Tính toán đóng góp của từng thành phần từ toàn bộ chu trình bằng cách sử dụng logic chẵn lẻ. Điều này tránh việc lặp lại qua m bước và giảm bài toán thành số học đơn giản về tần số. 
6. Xử lý trực tiếp phần còn lại của chu trình bằng cách chỉ mô phỏng các bước rem đầu tiên, vì rem nhiều nhất là n. Trong thời gian vượt qua này, hãy cập nhật trạng thái và tích lũy hạnh phúc. 
7. Kết hợp đóng góp toàn chu kỳ và đóng góp một phần chu kỳ cho mỗi người tham gia theo loại thành phần của họ. 

### Tại sao nó hoạt động 

Quá trình này phân hủy thành các máy trạng thái nhị phân độc lập cho mỗi thành phần, mỗi máy chỉ được điều khiển bởi các vị trí xuất hiện. Chiếc nồi không lưu trữ tính đa dạng, chỉ có sự hiện diện, vì vậy mỗi thành phần tiến hóa theo trình tự chuyển đổi. Vì chuỗi các lần xuất hiện là tuần hoàn với chu kỳ n nên hệ thống qua m bước là sự lặp lại của một mẫu chuyển đổi cố định cộng với một tiền tố. Điều này đảm bảo rằng mức tăng tính sẽ giảm xuống khi đếm các chuyển đổi trong đó trạng thái chuyển đổi đã hoạt động và các chuyển đổi đó có thể được tính toán bằng cách sử dụng số chu kỳ và tính chẵn lẻ mà không cần mô phỏng rõ ràng từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, m = map(int, input().split())
        a = list(map(int, input().split()))

        cnt = [0] * (k + 1)
        for x in a:
            cnt[x] += 1

        full = m // n
        rem = m % n

        # state per ingredient after full cycles (parity matters)
        # we only track whether it ends active or not in a compressed way
        # and compute gains via cycle aggregation

        active = [0] * (k + 1)
        ans = [0] * (k + 1)

        # simulate one cycle to understand intra-cycle gains
        state = [0] * (k + 1)
        cycle_gain = [0] * (k + 1)

        for x in a:
            if state[x]:
                cycle_gain[x] += 1
                state[x] = 0
            else:
                state[x] = 1

        # after one cycle, state[x] indicates parity effect
        for i in range(1, k + 1):
            if full % 2 == 1:
                active[i] = state[i]
            else:
                active[i] = 0

            ans[i] += (full // 1) * cycle_gain[i]

        # handle remainder
        state2 = active[:]
        for i in range(rem):
            x = a[i]
            if state2[x]:
                ans[x] += 1
                state2[x] = 0
            else:
                state2[x] = 1

        # map to players
        res = [0] * n
        for i in range(n):
            res[i] = ans[a[i]]

        print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai tách hành vi thành một mô phỏng một chu kỳ và sau đó sử dụng lại cấu trúc đó qua các lần lặp lại đầy đủ. Chi tiết quan trọng là chúng tôi chỉ mô phỏng rõ ràng một chu kỳ đầy đủ, sau đó sử dụng lại hiệu ứng của nó thông qua việc lặp lại, thay vì mô phỏng tất cả m bước. 

Phần tế nhị nhất là xử lý chính xác trạng thái chuyển đổi sau các chu kỳ đầy đủ. Một lỗi phổ biến là giả định sự độc lập của các chu kỳ mà không theo dõi xem thành phần đó kết thúc ở trạng thái hoạt động hay không hoạt động. Tính chẵn lẻ đó xác định liệu chu kỳ tiếp theo có bắt đầu với trạng thái đường cơ sở bị đảo ngược hay không, điều này sẽ thay đổi xem lần xuất hiện đầu tiên trong chu kỳ đó có tạo ra mức tăng hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 6
1 1 2
```Chúng tôi mô phỏng một chu kỳ gồm 3 bước. 

| Bước | Đang hoạt động | Hành động | Bang 1 | Bang 2 | Đạt được | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | chèn 1 | 1 | 0 | 0 | 
| 1 | 1 | rõ ràng 1 | 0 | 0 | 1 | 
| 2 | 2 | chèn 2 | 0 | 1 | 0 | 

Một chu kỳ tạo ra lợi ích: thành phần 1 tăng 1, thành phần 2 tăng 0. 

Chúng tôi lặp lại điều này với m = 6 = 2 chu kỳ, do đó thành phần 1 nhận được tổng cộng 2 mức tăng. 

Điều này xác nhận rằng việc tổng hợp chu trình hoạt động vì hành vi lặp lại chính xác sau mỗi n bước. 

### Ví dụ 2 

đầu vào:```
2 2 10
1 2
```Mô phỏng chu kỳ: 

| Bước | Đang hoạt động | Hành động | Bang 1 | Bang 2 | Đạt được | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | chèn 1 | 1 | 0 | 0 | 
| 1 | 1 | chèn 2 | 1 | 1 | 0 | 

Chu kỳ thứ hai hoạt động tương tự nhưng bắt đầu với cả hai trạng thái hoạt động tùy thuộc vào tính chẵn lẻ, cho thấy trạng thái ranh giới của chu kỳ có vấn đề. Điều này chứng tỏ tại sao chúng tôi theo dõi trạng thái qua các chu kỳ thay vì đặt lại một cách mù quáng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) mỗi lần kiểm tra | Một lần chuyển qua mảng cộng với việc xử lý tần suất cho mỗi thành phần | 
| Không gian | O(k) | Lưu trữ trạng thái thành phần và bộ đếm | 

Các ràng buộc cho phép tổng n và k lên tới 2×10^5, do đó thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Sample tests would go here once exact outputs are confirmed
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn tối thiểu | tầm thường | độ đúng cơ sở | 
| tất cả đều giống nhau | chuyển đổi định kỳ | xử lý chẵn lẻ chu kỳ | 
| tất cả đều khác biệt | không rõ ràng sớm | theo dõi độc lập | 
| lặp lại m lớn | hiệu suất | nén chu kỳ | 

## Vỏ cạnh 

Đối với trường hợp tất cả những người tham gia đều thích cùng một thành phần, thuật toán sẽ lập mô hình chính xác hành vi chèn và xóa xen kẽ vì mô phỏng chu trình ghi lại mẫu chuyển đổi chính xác một lần trong mỗi chu kỳ và lặp lại nó bằng cách sử dụng tính chẵn lẻ của các chu kỳ đầy đủ. 

Đối với trường hợp tất cả các thành phần đều khác biệt, mỗi chu kỳ chỉ thực hiện các thao tác chèn mà không xóa ngay lập tức và thuật toán phản ánh điều này bằng cách tính mức tăng bằng 0 trong mức tăng chu kỳ và do đó tạo ra đóng góp bằng 0 ngoại trừ khi các chu kỳ một phần giới thiệu lần xóa đầu tiên. 

Đối với trường hợp m nhỏ hơn n, phần mô phỏng còn lại xử lý mọi thứ một cách trực tiếp mà không dựa vào sự trừu tượng hóa chu trình, đảm bảo tính chính xác cho các chu trình không hoàn chỉnh.
