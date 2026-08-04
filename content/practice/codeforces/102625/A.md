---
title: "CF 102625A – Lời tạm biệt hoặc lời chúc tốt đẹp nhất"
description: "Lưới là một bảng hình chữ nhật có N hàng và M cột. Ô tô bắt đầu từ ô trên cùng bên trái và đi theo một lộ trình cố định: đầu tiên nó di chuyển dọc theo hàng đầu tiên cho đến khi đến góc trên cùng bên phải, sau đó di chuyển xuống dọc theo cột cuối cùng cho đến khi đến góc dưới cùng bên phải…"
date: "2026-08-03T15:24:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "A"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 59
verified: false
draft: false
---

[CF 102625A - Lời tạm biệt hoặc những lời chúc tốt đẹp nhất](https://codeforces.com/problemset/problem/102625/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một bảng hình chữ nhật có`N`hàng và`M`cột. Ô tô bắt đầu từ ô trên cùng bên trái và đi theo một tuyến đường cố định: đầu tiên nó di chuyển dọc theo hàng đầu tiên cho đến khi đến góc trên cùng bên phải, sau đó di chuyển xuống dọc theo cột cuối cùng cho đến khi đến góc dưới cùng bên phải. Mỗi giây, ô tô sẽ nhập ô tiếp theo. 

Bốn đặc vụ bắt đầu tại ô văn phòng`(X, Y)`. Hai trong số chúng chỉ di chuyển theo chiều dọc bên trong cột`Y`và hai cái còn lại chỉ di chuyển theo chiều ngang trong hàng`X`. Khi một tác nhân đến một biên giới, nó sẽ phản ánh và bắt đầu di chuyển theo hướng ngược lại. Câu hỏi đặt ra là liệu có tác nhân nào có thể chiếm cùng một ô với ô tô ở cùng một thời điểm hay không. 

Tọa độ có thể lớn bằng`10^9`, và có thể có tới`1000`trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mô phỏng. Một mô phỏng trực tiếp sẽ yêu cầu đi qua chiều dài đường đi, có thể gần bằng`2 * 10^9`giây cho một trường hợp thử nghiệm. Ngay cả một thuật toán tuyến tính cho mỗi trường hợp thử nghiệm cũng vượt xa giới hạn hai giây cho phép. Giải pháp phải sử dụng tính chất tuần hoàn của chuyển động phản ánh và giảm vấn đề về việc kiểm tra thời gian liên tục. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Một vụ va chạm có thể xảy ra ở thời điểm 0. Ví dụ:```
1
2 2 1 1
```Ô tô và đại lý bắt đầu từ cùng một ô, vì vậy câu trả lời là`BestWishes`. Một mô phỏng chỉ bắt đầu kiểm tra sau chuyển động đầu tiên sẽ chấp nhận nó một cách không chính xác. 

Một trường hợp khác là khi văn phòng nằm trên đường nhưng không ở điểm bắt đầu:```
1
3 3 2 3
```Các tác nhân dọc nằm ở cột thứ ba, đây chính xác là nơi ô tô di chuyển trong phần thứ hai của lộ trình. Bỏ qua đoạn cột vì hàng đầu tiên là tuyến đường chính sẽ bỏ sót va chạm. 

Tình huống khó khăn thứ ba là một bảng chỉ có hai hàng hoặc hai cột. Ví dụ:```
1
2 3 1 2
```Khoảng thời gian của một tác nhân trở nên rất nhỏ vì chỉ có một bước khả thi trước khi nảy sinh. Bất kỳ công thức nào giả định chu kỳ dài hơn hoặc chia cho 0 để tính toán khoảng thời gian sẽ không thành công. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là mô phỏng từng giây. Chúng ta có thể giữ vị trí và chỉ đạo của cả bốn đặc vụ, di chuyển mọi người và so sánh vị trí ô tô với mọi vị trí đặc vụ. Điều này đúng vì các quy tắc chuyển động có tính xác định và mọi va chạm có thể xảy ra tại một thời điểm nguyên nào đó trên tuyến đường. 

Vấn đề là độ dài tuyến đường. Nhu cầu tự động`(M - 1) + (N - 1)`giây, có thể đạt tới gần như`2 * 10^9`. Với bốn tổng đài viên, một trường hợp thử nghiệm có thể yêu cầu khoảng tám tỷ cập nhật vị trí. Điều này không thể vượt qua. 

Quan sát quan trọng là mọi tác nhân đều di chuyển trên một đường duy nhất và chuyển động nảy của nó lặp lại. Trên một dòng dài`L`, chu kì là`2 * (L - 1)`. Thay vì lưu trữ mọi vị trí, chúng tôi mô tả chuyển động bằng cách sử dụng tọa độ mở. Một người đi bộ bắt đầu ở vị trí`s`có định hướng`d`đã mở ra vị trí:`(s - 1) + d * t`lấy modulo thời gian. Hai người đi bộ có cùng một vị trí thực chính xác khi tọa độ mở của họ bằng nhau hoặc hình ảnh phản chiếu của nhau theo modulo chu kỳ. Điều này chuyển đổi việc phát hiện va chạm thành giải quyết một sự đồng đẳng tuyến tính nhỏ. 

Phương pháp vũ phu hoạt động vì nó kiểm tra rõ ràng mọi thời điểm. Quan sát về sự phản xạ tuần hoàn cho phép chúng ta thay thế hàng tỷ khoảnh khắc bằng một vài phép kiểm tra số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N + M) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hai chuyển động tác nhân dọc và hai chuyển động tác nhân ngang. Chuyển động thẳng đứng được thể hiện bằng hàng bắt đầu, hướng và độ dài đường`N`. Chuyển động ngang được thể hiện tương tự bằng cách sử dụng`M`. 
2. Kiểm tra va chạm trong phần đầu tiên của tuyến đường ô tô đi qua hàng`1`. Tác nhân dọc chỉ có thể va chạm ở đây vào thời điểm ô tô đến cột`Y`, vì vậy chúng tôi kiểm tra các tác nhân dọc vào thời điểm đó`Y - 1`. Một tác nhân ngang chỉ có thể va chạm trong toàn bộ phân khúc này nếu hàng văn phòng được`1`, vì vậy chúng tôi so sánh các tác nhân theo chiều ngang với chuyển động về phía đông của ô tô trên hàng. 
3. Kiểm tra va chạm ở đoạn đường thứ 2, ô tô di chuyển xuống cột`M`. Một tác nhân ngang chỉ có thể va chạm ở đây vào thời điểm ô tô đến hàng`X`, vì vậy chúng tôi sẽ kiểm tra nó vào lúc đó`M + X - 2`. Một tác nhân dọc chỉ có thể xung đột trong toàn bộ phân khúc này khi cột văn phòng được`M`, vì vậy chúng tôi so sánh các tác nhân theo chiều dọc với chuyển động về phía nam của ô tô. 
4. Nếu bất kỳ kiểm tra nào trong số này phát hiện xung đột, hãy in`BestWishes`. Nếu tất cả các lần kiểm tra đều không thành công, hãy in`Farewell`. 

Lý do điều này có tác dụng là vì mọi va chạm có thể xảy ra đều phải xảy ra trên một trong hai đoạn thẳng mà ô tô đang di chuyển. Trên mỗi đoạn, chỉ những tác nhân di chuyển trên cùng hàng hoặc cột với ô tô mới có thể giao nhau. Hàm va chạm định kỳ kiểm tra mọi thời gian gặp nhau có thể có trong tương lai trên đường một chiều đó, do đó không có khoảnh khắc nào bị bỏ qua. 

## Tại sao nó hoạt động 

Một khung tập đi phản xạ có thể được coi là chuyển động mãi mãi trên một đường thẳng chưa trải. Việc gấp đường gấp lại sẽ cho vị trí nảy thực tế. Hai khung tập đi ở cùng một vị trí gấp chính xác khi vị trí mở ra của chúng giống hệt nhau hoặc đối xứng quanh một điểm rẽ. Cả hai điều kiện đều trở thành phương trình mô đun với số lớp dư lượng có thể là không đổi. 

Thuật toán kiểm tra chính xác các lớp dư lượng đó đối với từng cặp chuyển động một chiều có thể giao nhau về mặt vật lý với tuyến đường ô tô. Vì mọi va chạm đều thuộc về một trong những trường hợp đó và mọi trường hợp như vậy đều được kiểm tra nên thuật toán sẽ trả về câu trả lời đúng.
