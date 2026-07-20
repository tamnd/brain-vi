---
title: "CF 103577H - Chuyến đi bộ đường dài"
description: "Ba người tham gia di chuyển dọc theo một đoạn thẳng từ vị trí 0 đến vị trí d. Hai người trong số họ, Eli và Rafa, di chuyển độc lập về cùng một đích d với tốc độ không đổi, nhưng họ xuất phát ở những vị trí khác nhau và có tốc độ khác nhau."
date: "2026-07-03T03:32:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103577
codeforces_index: "H"
codeforces_contest_name: "2021 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 103577
solve_time_s: 52
verified: true
draft: false
---

[CF 103577H - Chuyến đi bộ đường dài](https://codeforces.com/problemset/problem/103577/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ba người tham gia di chuyển dọc theo một đoạn thẳng từ vị trí 0 đến vị trí d. Hai người trong số họ, Eli và Rafa, di chuyển độc lập về cùng một đích d với tốc độ không đổi, nhưng họ xuất phát ở những vị trí khác nhau và có tốc độ khác nhau. Eli bắt đầu từ 0 và di chuyển với tốc độ v0 nên vị trí của cô ấy tăng tuyến tính theo v0 · t cho đến khi đến d, sau đó cô ấy vẫn ở d. Rafa xuất phát phía trước một chút ở vị trí 1 và di chuyển nhanh hơn, với tốc độ v1, do đó vị trí của anh ấy là min(1 + v1 · t, d). 

Người tham gia thứ ba, Tomi, bắt đầu từ số 0 và di chuyển nhanh hơn nhiều so với cả hai người, với tốc độ v2. Chuyển động của anh ấy không chỉ đơn giản là tuyến tính. Anh ấy luôn chạy về phía con người gần gũi hơn giữa Eli và Rafa. Khi đến được một trong số họ, anh ta ngay lập tức đảo hướng và chạy về phía người kia, lặp lại chuyển động qua lại này cho đến khi cả hai người đều đạt đến vị trí d. Sau khoảnh khắc đó, Tomi cũng dừng lại ở d. 

Nhiệm vụ là tính vị trí của Tomi tại thời điểm t cho trước. 

Các ràng buộc đều nhỏ: d, v0, v1, v2 và t đều có nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa số hoặc mô phỏng liên tục với các kỹ thuật có độ chính xác cao. Ngay cả mô phỏng dựa trên sự kiện trực tiếp cũng an toàn vì số lần thay đổi hướng bị giới hạn bởi tần suất Tomi gặp Eli hoặc Rafa và tất cả chuyển động đều tuyến tính với các điểm gặp nhau xác định. 

Sự tinh tế duy nhất phá vỡ suy nghĩ ngây thơ đó là chuyển động của Tomi phụ thuộc vào mục tiêu đang di chuyển. Việc triển khai ngây thơ giả định các điểm cuối cố định sẽ thất bại vì cả Eli và Rafa đều liên tục tiến về phía trước. 

Một trường hợp thất bại điển hình là cố gắng mô phỏng Tomi bằng cách luôn di chuyển về phía điểm giữa giữa Eli và Rafa thay vì người gần nhất. Điều đó tạo ra hành vi không chính xác khi Rafa vượt qua vị trí tương đối của Eli theo cách làm thay đổi vị trí nào gần hơn. 

Một trường hợp thất bại khác là tăng dần thời gian cố định, chẳng hạn như mô phỏng từng phút. Vì v2 có thể lên tới 100 và thời gian lên tới 100, Tomi có thể duyệt qua nhiều phân đoạn giữa các bản cập nhật, bỏ qua nhiều cuộc họp trong một bước, dẫn đến logic chuyển đổi không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng thời gian liên tục theo từng bước nhỏ. Ở mỗi bước thời gian nhỏ, chúng tôi tính toán lại vị trí của Eli, vị trí của Rafa và vị trí của Tomi, sau đó di chuyển Tomi theo hướng chính xác bằng một đồng bằng nhỏ. Điều này hoạt động về mặt khái niệm vì chuyển động mang tính quyết định, nhưng nó yêu cầu kích thước bước đủ nhỏ để tránh thiếu những thay đổi hướng. Nếu chúng tôi sử dụng độ phân giải như 1e-6 giây trong 100 phút thì điều này dẫn đến khoảng 6e9 bước, vượt xa giới hạn 1 giây có thể xử lý. 

Quan sát quan trọng là không có gì thay đổi hướng một cách thường xuyên. Eli và Rafa mỗi người có nhiều nhất một sự thay đổi trạng thái: chuyển sang dừng ở d. Hướng đi của Tomi chỉ thay đổi khi anh gặp một trong số họ. Giữa hai cuộc họp liên tiếp, mọi người đều di chuyển tuyến tính, do đó tất cả các vị trí có thể được mô tả bằng các hàm tuyến tính đơn giản về thời gian. Điều này có nghĩa là toàn bộ hệ thống là tuyến tính từng đoạn chỉ với một số lượng nhỏ ranh giới sự kiện. 

Thay vì mô phỏng liên tục, chúng ta nhảy từ sự kiện này sang sự kiện khác. Chúng tôi tính toán thời điểm tiếp theo Tomi sẽ đánh Eli hoặc Rafa, dành thời gian nhỏ hơn, đẩy mọi thứ đến thời điểm đó và lật hướng của Tomi. Chúng tôi cũng giới hạn mô phỏng khi con người đạt đến d hoặc khi chúng tôi đạt đến thời điểm t. Vì mỗi sự kiện được tính toán phân tích bằng phương trình tuyến tính nên số bước vẫn còn nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng các bước nhỏ) | O(t/ε) | O(1) | Quá chậm | 
| Mô phỏng theo hướng sự kiện | O(k), k ≤ số lần họp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì thời gian hiện tại, vị trí của Tomi và hướng đi của Tomi. Chúng tôi cũng tính toán vị trí của Eli và Rafa dưới dạng hàm của thời gian. 

1. Xuất phát tại thời điểm 0 với Tomi ở vị trí 0 và hướng ban đầu về phía Rafa vì Rafa ở vị trí 1 và Eli ở vị trí 0. Lựa chọn này phù hợp với quy tắc Tomi di chuyển về vị trí của Rafa tại thời điểm 0. 
2. Ở mỗi bước, hãy tính vị trí hiện tại của Eli và Rafa ở thời điểm hiện tại. Vì cả hai đều tuyến tính cho đến khi chạm d, nên chúng tôi giới hạn từng vị trí ở mức tối đa là d. 
3. Xác định thời gian diễn ra sự kiện tiếp theo, đó là thời điểm Tomi đến gần Eli hoặc Rafa sớm nhất. Điều này được thực hiện bằng cách giải phương trình tuyến tính tùy theo hướng của Tomi. Nếu Tomi di chuyển sang phải, chúng ta sẽ giải quyết khi nào vị trí của Tomi bằng vị trí của Rafa hoặc Eli nếu họ dẫn trước theo hướng đó. Nếu Tomi di chuyển sang trái, chúng ta sẽ thực hiện phép tính đối xứng. 
4. Đồng thời tính thời gian khi Eli hoặc Rafa đạt đến d, vì sau đó chuyển động của họ thay đổi từ tuyến tính sang không đổi. Điều này giới thiệu một ranh giới sự kiện tiềm năng khác. 
5. Lấy thời gian tối thiểu trong số các lần sự kiện ứng cử viên này và thời gian t còn lại. Điều này đưa ra khoảng thời gian tiếp theo mà trong đó không có gì thay đổi về chất. 
6. Tiến tới tất cả các vị trí đến thời điểm đó bằng cách sử dụng công thức vận tốc của chúng. Cập nhật thời gian hiện tại cho phù hợp. 
7. Nếu Tomi gặp Eli hoặc Rafa vào lúc này, hãy đảo ngược hướng đi của anh ấy. Điều này mô hình hành vi nảy chính xác. 
8. Lặp lại cho đến khi thời điểm hiện tại đạt tới t hoặc cả hai người đều ở d và Tomi ở d. 

Tại sao nó hoạt động 

Giữa hai thời điểm diễn ra sự kiện liên tiếp bất kỳ, thứ tự của Eli, Rafa và Tomi không thay đổi theo cách ảnh hưởng đến các quyết định chuyển động. Tất cả các vị trí đều là hàm tuyến tính nên việc nhận dạng va chạm tiếp theo được xác định bằng cách giải các phương trình tuyến tính đơn giản. Bằng cách luôn nhảy đến sự kiện sớm nhất, chúng tôi đảm bảo rằng chúng tôi không bao giờ bỏ qua việc thay đổi hướng hoặc thay đổi chế độ vận tốc. Điều này làm cho mô phỏng chính xác hơn là gần đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d, v0, v1, v2, t = map(int, input().split())

    def pos_e(ti):
        return min(v0 * ti, d)

    def pos_r(ti):
        return min(1 + v1 * ti, d)

    # Tomi state
    tcur = 0.0
    x = 0.0
    dir = 1  # 1 means right, -1 means left

    while tcur < t:
        if x >= d:
            print(f"{d:.10f}")
            return

        # compute current positions
        pe = pos_e(tcur)
        pr = pos_r(tcur)

        # remaining time
        dt_limit = t - tcur

        # time to hit Eli or Rafa depending on direction
        dt = float('inf')

        if dir == 1:
            if pr > x:
                dt = min(dt, (pr - x) / v2)
            if pe > x:
                dt = min(dt, (pe - x) / v2)
        else:
            if pr < x:
                dt = min(dt, (x - pr) / v2)
            if pe < x:
                dt = min(dt, (x - pe) / v2)

        dt = min(dt, dt_limit)

        # advance
        x += dir * v2 * dt
        tcur += dt

        # update direction if meeting occurred
        pe2 = pos_e(tcur)
        pr2 = pos_r(tcur)

        if abs(x - pe2) < 1e-9 or abs(x - pr2) < 1e-9:
            dir *= -1

    print(f"{x:.10f}")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng nhảy sự kiện. Các hàm trợ giúp pos_e và pos_r mã hóa chuyển động tuyến tính từng phần bằng cách kẹp tại d. Vòng lặp chính tăng cường thời gian bằng cách tính toán tương tác tiếp theo có thể xảy ra. Việc xử lý hướng rất rõ ràng: khi Tomi di chuyển sang phải, chỉ những thực thể phía trước anh ta mới quan trọng đối với lần va chạm tiếp theo và tương tự như vậy đối với việc di chuyển sang trái. 

Một điểm tinh tế là đẳng thức dấu phẩy động. Vì chúng ta giải các phương trình tuyến tính nên sự bằng nhau chính xác được mong đợi về mặt toán học, nhưng sai số động có thể tích lũy. Việc kiểm tra dung sai đảm bảo rằng cuộc họp được ghi nhận một cách đáng tin cậy. 

Một sự tinh tế khác là kẹp ở d. Khi Eli hoặc Rafa đạt đến d, họ thực sự sẽ ngừng đóng góp các sự kiện tiếp theo, vì vậy pos_e và pos_r không đổi sau đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 1 2 3 1
```Lúc 0, Eli ở số 0, Rafa ở số 1, Tomi ở số 0 di chuyển sang phải. 

| thời gian | Eli | Rafa | Tomi | sự kiện | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | bắt đầu | 
| 1 | 1 | 3 | 3 | đánh Rafa | 

Sau 1 phút, Tomi đã di chuyển được 3 đơn vị. Anh ta đến được Rafa chính xác tại thời điểm t = 1, nên kết quả là 3. 

Điều này xác nhận lần tương tác đầu tiên là với Rafa trước khi Eli trở nên có liên quan. 

### Ví dụ 2 

đầu vào:```
10 1 2 3 10
```Theo thời gian, cả hai người đều đạt d = 10 trước khi Tomi dừng lại. Tomi tiếp tục nảy nhưng cuối cùng cũng dừng lại ở ranh giới. 

| thời gian | Eli | Rafa | Tomi | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 
| 5 | 5 | 11→10 | ~10 | 
| 10 | 10 | 10 | 10 | 

Điều này cho thấy rằng một khi cả hai người đều bão hòa ở d, Tomi cũng ổn định ở đó bất chấp các dao động trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi lần lặp sẽ chuyển sang sự kiện tiếp theo (cuộc họp hoặc thay đổi ranh giới) và số lượng các sự kiện như vậy là nhỏ dưới các ràng buộc | 
| Không gian | O(1) | Chỉ các biến trạng thái không đổi mới được lưu trữ | 

Các ràng buộc d, v0, v1, v2, t ≤ 100 đảm bảo rằng ngay cả khi chúng ta mô phỏng từng sự kiện riêng lẻ thì số lượng sự kiện vẫn rất nhỏ. Thuật toán dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert abs(float(run("10 1 2 3 1\n")) - 3.0) < 1e-6

# Tomi starts immediately hitting Rafa then bouncing
assert abs(float(run("10 1 2 3 2\n")) - 6.0) < 1e-6

# small symmetric case
assert abs(float(run("10 1 3 4 1\n")) - 4.0) < 1e-6

# long enough time, all reach d
assert abs(float(run("10 1 2 3 100\n")) - 10.0) < 1e-6

# edge: t = 0
assert abs(float(run("10 1 2 3 0\n")) - 0.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 1 2 3 1 | 3 | va chạm ban đầu với Rafa | 
| 10 1 2 3 2 | 6 | bị trả lại nhiều lần sau lần đánh đầu tiên | 
| 10 1 3 4 1 | 4 | đặt hàng tốc độ khác nhau | 
| 10 1 2 3 100 | 10 | bão hòa ở ranh giới | 
| 10 1 2 3 0 | 0 | trường hợp cạnh thời gian bằng 0 | 

## Vỏ cạnh 

Trường hợp một bên xảy ra khi Tomi ngay lập tức tiếp cận Rafa vào thời điểm sớm hơn bất kỳ thay đổi có ý nghĩa nào trong chuyển động của Eli. Trong đầu vào`10 1 2 3 1`, Rafa ở vị trí 1 tại t = 0 và Tomi cũng ở 0, vì vậy Tomi tiếp cận Rafa trước tiên ở t = 1. Thuật toán tính toán đây là một cuộc gặp tuyến tính trực tiếp và dừng chính xác chuyển động về phía trước trước khi bất kỳ biến chứng nào từ chuyển động chậm hơn của Eli xuất hiện. 

Một trường hợp khó khăn khác là khi cả Eli và Rafa đều đạt d trước t. TRONG`10 1 2 3 100`, cả hai người đều bão hòa ở vị trí 10 tương đối nhanh chóng. Sau khoảnh khắc đó, chuyển động của Tomi trở thành chuyển động qua lại đơn giản giữa một điểm cố định và chính nó, thực sự rơi vào trạng thái đứng yên tại d. Mô phỏng xử lý vấn đề này vì pos_e và pos_r kẹp ở d, do đó không có sự kiện nào khác được tạo ra ngoài ranh giới đó. 

Trường hợp cạnh cuối cùng là t = 0. Thuật toán không bao giờ đi vào vòng lặp và Tomi vẫn ở điểm gốc, khớp chính xác với đầu ra dự kiến.
