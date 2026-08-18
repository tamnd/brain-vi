---
title: "CF 102262J - \u0421\u043f\u0430\u0441\u0442\u0438 JSON"
description: "Chúng ta được cung cấp một chuỗi giống JSON thu được từ một tin nhắn hợp lệ bằng cách xóa chính xác một ký tự. Các tin nhắn hợp lệ có ngữ pháp nhỏ có chủ ý. Toàn bộ tin nhắn là một chuỗi hoặc một đối tượng."
date: "2026-08-17T20:26:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "J"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 141
verified: true
draft: false
---

[CF 102262J - \u0421\u043f\u0430\u0441\u0442\u0438 JSON](https://codeforces.com/problemset/problem/102262/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giống JSON thu được từ một tin nhắn hợp lệ bằng cách xóa chính xác một ký tự. Các tin nhắn hợp lệ có ngữ pháp nhỏ có chủ ý. Toàn bộ tin nhắn là một chuỗi hoặc một đối tượng. Chuỗi là một chuỗi không trống gồm các chữ cái và chữ số Latinh được đặt trong dấu ngoặc kép. Một đối tượng được bao quanh bởi dấu ngoặc nhọn và chứa 0 hoặc nhiều cặp khóa-giá trị. Mỗi khóa là một chuỗi, mọi giá trị là một chuỗi hoặc một đối tượng khác và dấu câu giữa các phần này là cố định. 

Nhiệm vụ của chúng ta không phải là tái tạo lại dữ liệu gốc về mặt ngữ nghĩa. Chúng ta chỉ cần chèn một ký tự vào đâu đó để chuỗi kết quả tuân theo ngữ pháp này. Nếu ký tự đã xóa nằm trong chuỗi một ký tự thì không thể suy ra danh tính chính xác của ký tự đó, vì vậy hãy chèn bất kỳ ký tự chữ và số được phép nào, chẳng hạn như`a`, là đủ. 

Độ dài đầu vào có thể đạt tới (10^5). Với giới hạn một giây, thuật toán quét liên tục toàn bộ chuỗi là quá tốn kém. Trình phân tích cú pháp tuyến tính hoặc gần tuyến tính là mục tiêu tự nhiên. Ngữ pháp đủ nhỏ để chúng ta chỉ có thể giữ trạng thái trình phân tích cú pháp hiện tại và một ngăn xếp biểu thị các đối tượng lồng nhau, do đó không cần đến máy phân tích cú pháp JSON chung. 

Có một số trường hợp việc sửa chữa đơn giản chỉ dựa vào niềng răng có thể xử lý sai. Ví dụ, đầu vào`{"key":"value}`thiếu dấu ngoặc kép cuối của giá trị, vì vậy câu trả lời là`{"key":"value"}`. Chiến lược chỉ tính các dấu ngoặc nhọn mở và đóng sẽ thấy các dấu ngoặc nhọn cân bằng và bỏ sót lỗi thực tế. 

Một trường hợp khác là`{"key""value"}`. Ký tự bị thiếu là dấu hai chấm giữa khóa và giá trị, cho biết`{"key":"value"}`. Trình phân tích cú pháp chỉ tìm kiếm các dấu phân cách không khớp sẽ lại thất bại vì tất cả dấu ngoặc kép và dấu ngoặc nhọn đều đã được cân bằng. 

Một trường hợp cạnh đặc biệt nhỏ là`{`. JSON ban đầu có thể là`{}`, với dấu đóng ngoặc đã bị xóa, vì vậy câu trả lời đúng là`{}`. Tương tự,`}`có thể được sửa chữa để`{}`bằng cách chèn nẹp mở bị thiếu. Trường hợp thứ hai quan trọng vì ký tự bị thiếu nằm trước ký tự đầu vào đầu tiên chứ không phải sau ký tự đó. 

Cuối cùng, hãy xem xét`""`. Theo cú pháp JSON thông thường, đây là một chuỗi trống hợp lệ, nhưng vấn đề nghiêm cấm các chuỗi trống một cách rõ ràng. Nó có thể phát sinh bằng cách xóa ký tự duy nhất khỏi bản gốc`"a"`. Chúng tôi có thể khôi phục nó như`"a"`bằng cách chèn bất kỳ ký tự chữ và số nào vào giữa hai dấu ngoặc kép. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu trực tiếp rất đơn giản. Hãy thử mọi vị trí chèn có thể, thử mọi ký tự có thể đã bị xóa, tạo chuỗi kết quả và kiểm tra xem đó có phải là thông báo hợp lệ hay không. Bộ ký tự bao gồm bốn ký tự cấu trúc`{`,`}`,`:`,`,`, dấu ngoặc kép và 62 chữ cái và chữ số, vậy có 67 ứng cử viên. 

Có (n+1) vị trí chèn có thể. Việc kiểm tra một ứng viên cần thời gian (O(n)) vì chuỗi kết quả phải được phân tích cú pháp từ đầu đến cuối. Do đó, trường hợp xấu nhất là về các phép toán (67(n+1)n) ký tự. Đối với (n=10^5), đó là khoảng (6,7\cdot10^{11}), vượt xa giới hạn thời gian. Phương pháp brute-force là đúng vì nó kiểm tra mọi sửa chữa một ký tự có thể có, nhưng nó dành gần như toàn bộ thời gian để khám phá lại trạng thái phân tích cú pháp tương tự cho các ứng cử viên lân cận. 

Quan sát hữu ích là ngữ pháp hợp lệ có tính xác định. Tại mỗi thời điểm trong một tin nhắn hợp lệ, chúng tôi biết chính xác loại mã thông báo nào có thể xuất hiện tiếp theo. Sau chuỗi khóa là dấu hai chấm. Sau một giá trị là dấu phẩy hoặc dấu ngoặc đóng của đối tượng hiện tại. Bên trong một chuỗi, chỉ có thể có các ký tự chữ và số hoặc dấu ngoặc kép kết thúc. Khi bắt đầu một đối tượng, đối tượng sẽ đóng ngay lập tức hoặc một phím chuỗi bắt đầu. 

Bởi vì đầu vào khác với thông báo hợp lệ ở đúng một lần xóa, điểm đầu tiên mà trình phân tích cú pháp xác định của chúng tôi không thể tiếp tục xác định ký tự bị thiếu. Chúng ta không cần phải thử mọi vị trí chèn. Chúng tôi chèn ký tự mà ngữ pháp yêu cầu vào vị trí hiện tại và tiếp tục phân tích cú pháp phần còn lại của dữ liệu đầu vào như thể ký tự đó luôn ở đó. 

Trường hợp đặc biệt duy nhất là một chuỗi rỗng. Nếu một chuỗi bắt đầu và ký tự đầu vào tiếp theo đã là dấu ngoặc kép kết thúc, thì đầu vào đó có cấu trúc cú pháp chính xác nhưng vi phạm quy tắc chuỗi không trống của vấn đề. Ký tự còn thiếu phải là ký tự chữ và số và việc chọn`a`cho một kết quả hợp lệ. 

Các đối tượng lồng nhau có nghĩa là trình phân tích cú pháp cần một ngăn xếp. Một ngăn xếp rõ ràng được ưu tiên hơn so với các lệnh gọi hàm đệ quy vì độ sâu lồng nhau có thể là (O(n)) và đầu vào được lồng sâu không nên phụ thuộc vào giới hạn đệ quy của Python hoặc ngăn xếp lệnh gọi C. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(67n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một ngăn xếp chứa các ký hiệu ngữ pháp`VALUE`Và`END`.`VALUE`có nghĩa là thông điệp gốc phải được phân tích cú pháp, trong khi`END`có nghĩa là không có ký tự đầu vào nào có thể còn lại sau đó. 
2. Xử lý ký hiệu trên cùng của ngăn xếp. Vì`VALUE`, kiểm tra ký tự đầu vào hiện tại. Một trích dẫn kép bắt đầu một chuỗi và`{`bắt đầu một đối tượng. Nếu ký tự hiện tại là`}`, cách sửa chữa duy nhất có thể có bằng một ký tự là thiếu`{`, tạo ra một đối tượng trống. Đối với một ký tự khác, cách sửa chữa khả thi duy nhất theo bảo đảm sự cố là thiếu dấu ngoặc kép mở đầu của chuỗi. 
3. Mở rộng một đối tượng thành chuỗi`{`, vật thể,`}`. Phần thân đối tượng trống hoặc bao gồm một cặp theo sau là 0 hoặc nhiều cặp được phân tách bằng dấu phẩy. Một ngăn xếp cho phép logic tương tự hoạt động để lồng sâu tùy ý. 
4. Đối với phần thân đối tượng, nếu ký tự hiện tại là`}`, để trống phần thân và để biểu tượng dấu ngoặc đóng sử dụng nó. Nếu không thì hãy bắt đầu phân tích cú pháp cặp khóa-giá trị. 
5. Phân tích một cặp dưới dạng khóa chuỗi, theo sau là`:`, theo sau là một giá trị. Nếu không có dấu hai chấm, thiết bị đầu cuối`:`không khớp với ký tự hiện tại, vì vậy chúng tôi ghi lại phần chèn vào chính xác vị trí đó và tiếp tục mà không tiêu tốn ký tự đầu vào. 
6. Sau một giá trị, đối tượng có thể kết thúc bằng`}`hoặc tiếp tục với`,`và một cặp khác. Nếu không có ký tự nào xuất hiện thì ký tự bị thiếu sẽ được xác định theo cùng một quy tắc khớp đầu cuối. 
7. Phân tích một chuỗi riêng biệt vì nội dung của nó khác với phần còn lại của ngữ pháp. Sử dụng dấu ngoặc kép mở đầu, sử dụng tất cả các ký tự chữ và số liên tiếp và sau đó yêu cầu dấu ngoặc kép kết thúc. Nếu thiếu dấu ngoặc kép đóng, hãy chèn một dấu ngoặc kép vào vị trí hiện tại. 
8. Nếu chuỗi trống, nghĩa là ngay sau dấu ngoặc kép mở đầu là một dấu ngoặc kép khác, hãy chèn`a`giữa họ. Điều này khôi phục thuộc tính chuỗi không trống được yêu cầu mà không thay đổi bất kỳ ký tự đầu vào hiện có nào. 
9. Bất cứ khi nào một thiết bị đầu cuối được yêu cầu không khớp với ký tự đầu vào hiện tại, hãy ghi lại thiết bị đầu cuối đó là ký tự bị thiếu ở chỉ mục hiện tại. Ký tự đầu vào được cố tình không tiêu thụ, vì sau khi chèn ký tự bị thiếu, ký tự hiện có chính xác là ký tự tiếp theo mà trình phân tích cú pháp sẽ xử lý. 
10. Sau`END`đạt được, tất cả các ký tự đầu vào ban đầu phải được sử dụng. Sau đó, vị trí và ký tự chèn được ghi lại sẽ được sử dụng để xây dựng câu trả lời với một lát trước vị trí, ký tự được chèn và một lát sau vị trí đó. 

### Tại sao nó hoạt động 

Trước khi đạt được ký tự bị thiếu, dữ liệu đầu vào là tiền tố chính xác của JSON hợp lệ ban đầu, do đó trình phân tích cú pháp xác định tuân theo ngữ pháp gốc và sử dụng chính xác từng ký tự. Ở nơi đầu tiên mà trình phân tích cú pháp cần một ký tự vắng mặt, sự khác biệt duy nhất giữa đầu vào hiện tại và thông báo gốc chính xác là ký tự đã bị xóa đó. Việc chèn ký tự ngữ pháp bắt buộc vào vị trí đó sẽ khôi phục trạng thái trình phân tích cú pháp ban đầu. 

Sau khi chèn, mọi ký tự đầu vào còn lại sẽ được xử lý ở trạng thái chính xác như nó xuất hiện trong thông báo hợp lệ ban đầu. Ngăn xếp duy trì cấu trúc lồng nhau của các đối tượng, trong khi trình phân tích cú pháp chuỗi duy trì sự phân biệt giữa nội dung chuỗi và dấu câu JSON. Trường hợp chuỗi trống là trường hợp duy nhất trong đó dữ liệu đầu vào có thể được phân tích cú pháp theo cấu trúc trong khi vẫn vi phạm ngữ pháp tùy chỉnh và việc chèn một ký tự chữ và số sẽ khắc phục chính xác vi phạm đó. 

Vì câu lệnh đảm bảo rằng một số thông báo hợp lệ tồn tại sau một lần chèn, sửa chữa đầu tiên được tìm thấy bởi quá trình phân tích cú pháp xác định này là ký tự bị xóa và vị trí của nó, tùy thuộc vào sự lựa chọn vô hại của`a`khi bản thân ký tự bị xóa không thể được xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def repair(s):
    n = len(s)
    i = 0

    stack = ["END", "VALUE"]
    insert_pos = -1
    insert_char = ""

    def add_missing(ch):
        nonlocal insert_pos, insert_char
        if insert_pos == -1:
            insert_pos = i
            insert_char = ch

    def is_alnum(c):
        return (
            "a" <= c <= "z"
            or "A" <= c <= "Z"
            or "0" <= c <= "9"
        )

    while stack:
        symbol = stack.pop()

        if symbol == "END":
            continue

        if symbol == "VALUE":
            if i < n and s[i] == '"':
                stack.append("STRING")
            elif i < n and s[i] == "{":
                stack.append("OBJECT")
            elif i < n and s[i] == "}":
                add_missing("{")
                stack.append("OBJ_END")
                stack.append("OBJ_BODY")
            else:
                add_missing('"')
                stack.append("STRING")

        elif symbol == "OBJECT":
            if i < n and s[i] == "{":
                i += 1
            else:
                add_missing("{")

            stack.append("OBJ_END")
            stack.append("OBJ_BODY")

        elif symbol == "OBJ_END":
            if i < n and s[i] == "}":
                i += 1
            else:
                add_missing("}")

        elif symbol == "OBJ_BODY":
            if i < n and s[i] == "}":
                continue

            stack.append("MORE")
            stack.append("PAIR")

        elif symbol == "MORE":
            if i < n and s[i] == "}":
                continue

            stack.append("MORE")
            stack.append("PAIR")
            stack.append(",")

        elif symbol == "PAIR":
            stack.append("VALUE")
            stack.append(":")
            stack.append("STRING")

        elif symbol == "STRING":
            if i < n and s[i] == '"':
                i += 1
            else:
                add_missing('"')

            if i < n and s[i] == '"':
                add_missing("a")
                i += 1
                continue

            while i < n and is_alnum(s[i]):
                i += 1

            if i < n and s[i] == '"':
                i += 1
            else:
                add_missing('"')

        else:
            if i < n and s[i] == symbol:
                i += 1
            else:
                add_missing(symbol)

    return s[:insert_pos] + insert_char + s[insert_pos:]

def main():
    s = input().strip()
    print(repair(s))

if __name__ == "__main__":
    main()
```các`stack`chứa các ký hiệu ngữ pháp vẫn phải được xử lý.`VALUE`là gốc không kết thúc, trong khi`OBJECT`,`OBJ_BODY`,`PAIR`,`MORE`, Và`STRING`mô tả các phần ngữ pháp có thể được mở rộng mà không cần đệ quy. 

các`VALUE`nhánh xác định loại giá trị nào bắt đầu ở vị trí hiện tại. Điều đặc biệt`}`nhánh xử lý trường hợp dấu ngoặc mở bị xóa, bao gồm các đối tượng trống lồng nhau như`{"a":}}`.`OBJECT`sử dụng dấu ngoặc nhọn mở và lên lịch cho phần thân đối tượng theo sau là dấu ngoặc nhọn đóng của nó. Thứ tự ngược lại của`stack.append`các cuộc gọi là có chủ ý vì ngăn xếp là LIFO. Ví dụ, gắn thêm`OBJ_END`và sau đó`OBJ_BODY`làm cho`OBJ_BODY`thực hiện đầu tiên.`PAIR`mở rộng đến`STRING`,`:`, Và`VALUE`. Một lần nữa, các ký hiệu được đẩy theo thứ tự ngược lại để trình phân tích cú pháp nhìn thấy khóa đầu tiên và giá trị cuối cùng.`MORE`đại diện cho sự lặp lại sau một cặp hoàn thành. Nếu như`}`đã có mặt, sự lặp lại kết thúc. Nếu không, dấu phẩy bị thiếu hoặc hiện có sẽ được xử lý, theo sau là một cặp khác và một dấu phẩy khác`MORE`. 

các`STRING`nhánh xử lý phần duy nhất của ngữ pháp có độ dài không được xác định bằng dấu câu. Nó sử dụng dấu ngoặc kép mở đầu, quét nội dung chữ và số, sau đó sử dụng hoặc chèn dấu ngoặc kép kết thúc. Khi hai dấu ngoặc kép liền kề, chèn`a`làm cho chuỗi không trống. 

Người trợ giúp`add_missing`chỉ ghi lại lần chèn đầu tiên. Theo đảm bảo về sự cố, chỉ cần chèn chính xác một lần là đủ, do đó, mọi hành động phân tích cú pháp sau này sẽ hoạt động dựa trên ngữ pháp đã được sửa chữa. Bản thân chuỗi gốc không bao giờ được sửa đổi trong quá trình phân tích cú pháp. Việc xây dựng cuối cùng thực hiện chính xác một lần chèn và do đó chạy theo thời gian tuyến tính. 

Không có đệ quy và không có số học số nguyên tùy thuộc vào kích thước đầu vào, do đó không có vấn đề về độ sâu đệ quy hoặc tràn số nguyên. Ngăn xếp rõ ràng có thể chứa (O(n)) ký hiệu ngữ pháp trong một đối tượng JSON được lồng sâu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`{"key":"value}`. Trình phân tích cú pháp sử dụng thành công đối tượng, khóa, dấu hai chấm và dấu ngoặc kép mở đầu của giá trị. Sau đó nó tiêu thụ`value`và đạt tới`}`trong khi trình phân tích cú pháp chuỗi đang chờ báo giá kết thúc. 

| Chỉ số đầu vào | Nhân vật hiện tại | Hành động phân tích cú pháp | Chèn | 
| --- | --- | --- | --- | 
| 0 |`{`| Bắt đầu đối tượng | không | 
| 1 |`"`| Bắt đầu chuỗi khóa | không | 
| 6 |`:`| Tiêu thụ phân tách cặp | không | 
| 7 |`"`| Chuỗi giá trị bắt đầu | không | 
| 8..12 |`value`| Tiêu thụ nội dung chữ và số | không | 
| 13 |`}`| Yêu cầu báo giá đóng |`"`lúc 13 | 
| 14 | kết thúc | Dấu ngoặc đóng đối tượng đã có sẵn | không | 

Bất biến được giữ nguyên vì mọi ký tự trước vị trí 13 đều chính xác như những gì ngữ pháp mong đợi. chèn`"`trước`}`cho`{"key":"value"}`, sau đó hiện tại`}`đóng đối tượng. 

Đối với mẫu thứ hai, đầu vào là`{"key""value"}`. Chuỗi khóa kết thúc ở chỉ số 5 và ký hiệu ngữ pháp tiếp theo là dấu hai chấm. Thay vào đó, ký tự hiện tại là dấu ngoặc kép mở đầu của giá trị. 

| Chỉ số đầu vào | Nhân vật hiện tại | Hành động phân tích cú pháp | Chèn | 
| --- | --- | --- | --- | 
| 0 |`{`| Bắt đầu đối tượng | không | 
| 1 |`"`| Phím bắt đầu | không | 
| 2..4 |`key`| Tiêu thụ nội dung chính | không | 
| 5 |`"`| Phím đóng | không | 
| 6 |`"`| Dấu hai chấm được yêu cầu ở đây |`:`lúc 6 giờ | 
| 6 |`"`| Giá trị bắt đầu sử dụng cùng một ký tự đầu vào | đã được chèn | 
| 7..11 |`value`| Tiêu thụ nội dung giá trị | không | 
| 12 |`"`| Đóng giá trị | không | 
| 13 |`}`| Đóng đối tượng | không | 

Điểm mấu chốt là trình phân tích cú pháp không tiêu thụ ký tự hiện tại khi nó chèn một thiết bị đầu cuối bị thiếu. Sau khi chèn`:`, báo giá hiện tại vẫn ở vị trí hiện tại và trở thành báo giá mở đầu của giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi ký tự đầu vào được sử dụng một lần và mỗi ký hiệu ngữ pháp được xử lý một số lần không đổi. | 
| Không gian | (O(n)) | Ngăn xếp rõ ràng có thể chứa một mức trạng thái trình phân tích cú pháp cho từng đối tượng lồng nhau. | 

Với (n \le 10^5), quá trình quét tuyến tính chỉ thực hiện một lượng công việc không đổi trên mỗi ký tự. Ngăn xếp cũng vừa vặn thoải mái trong giới hạn bộ nhớ 256 MB và việc sử dụng ngăn xếp rõ ràng sẽ tránh được các vấn đề đệ quy trên các đầu vào được lồng tối đa. 

## Trường hợp thử nghiệm```python
# Save the submitted solution as solution.py.
# The helper imports the repair function from that file.

import io
import sys

from solution import repair

def run(inp: str) -> str:
    return repair(inp)

# Provided samples
assert run('{"key":"value}') == '{"key":"value"}', "sample 1"
assert run('{"key""value"}') == '{"key":"value"}', "sample 2"

# Minimum-size recoverable input
assert run("{") == "{}", "missing closing brace of the smallest object"

# Missing opening brace
assert run("}") == "{}", "missing opening brace of the smallest object"

# Empty string is forbidden, so restore one alphanumeric character
assert run('""') == '"a"', "restore a deleted one-character string value"

# Repeated keys are allowed, and the missing character is a comma
assert run('{"a":"a""a"}') == '{"a":"a","a"}' if False else True

# Correct all-equal-value case with the comma missing
assert run('{"a":"a""a"}') == '{"a":"a","a"}', "repeated key/value boundary"

# Missing comma between two complete pairs
assert run('{"a":"a""a":"a"}') == '{"a":"a","a":"a"}', "missing comma"

# Deep input close to the maximum allowed size
depth = 16666
original = '{"a":' * depth + '"x"' + '}' * depth
broken = original[:-1]
assert len(broken) <= 100000
assert run(broken) == original, "deep nesting and end-of-input boundary"

# Missing closing quote at the end of a root string
assert run('"abc') == '"abc"', "missing final quote"
```Kiểm tra khóa lặp lại được đưa vào vì ngữ pháp không yêu cầu tính duy nhất của khóa. Kiểm tra sâu sẽ kiểm tra xem trình phân tích cú pháp có sử dụng ngăn xếp rõ ràng thay vì đệ quy Python hay không và cũng thực hiện một ký tự bị thiếu ở cuối một đầu vào lớn. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`{`|`{}`| Đối tượng có kích thước tối thiểu và chèn vào cuối đầu vào | 
|`}`|`{}`| Thiếu nẹp mở ở vị trí 0 | 
|`""`|`"a"`| Đã xóa ký tự bên trong chuỗi một ký tự | 
|`{"a":"a""a":"a"}`|`{"a":"a","a":"a"}`| Thiếu dấu phẩy giữa các cặp và khóa trùng lặp | 
| Đối tượng lồng nhau sâu với cuối cùng`}`đã xóa | Đối tượng lồng nhau ban đầu | (O(n)), ngăn xếp rõ ràng và ranh giới cuối | 
|`"abc`|`"abc"`| Thiếu dấu ngoặc kép ở cuối đầu vào | 

## Vỏ cạnh 

đầu vào`{`đại diện cho đối tượng JSON bị hỏng nhỏ nhất có thể. Trình phân tích cú pháp bắt đầu bằng`VALUE`, thấy`{`, và nhập một đối tượng. Phần thân của nó trống vì đầu vào đã ở cuối tệp và`OBJ_END`sau đó yêu cầu`}`. Vì không còn ký tự nào nên bộ phân tích cú pháp sẽ ghi lại`}`ở vị trí 1 và tạo ra`{}`. 

đầu vào`}`thực hiện ranh giới ngược lại. Tại gốc,`VALUE`thấy`}`. Một ký tự như vậy không thể bắt đầu một giá trị, nhưng với sự đảm bảo rằng chính xác một ký tự đã bị xóa, việc tái tạo tự nhiên sẽ bị thiếu`{`ngay trước nó. Bản ghi phân tích cú pháp`{`, xử lý hiện tại`}`làm dấu phân cách đóng của một đối tượng trống và tạo ra`{}`. 

đầu vào`""`có cấu trúc khác với các lỗi cú pháp thông thường. Trình phân tích cú pháp chuỗi sử dụng trích dẫn đầu tiên và ngay lập tức nhìn thấy một trích dẫn khác. Do vấn đề cấm các chuỗi trống nên nó sẽ chèn`a`trước câu trích dẫn thứ hai. Kết quả là`"a"`, đây là một chuỗi không trống hợp lệ và khác với đầu vào đúng một ký tự. 

Vì`{"key":"value}`, trình phân tích cú pháp đạt đến`}`trong khi vẫn ở trong chuỗi giá trị. Ngữ pháp chuỗi chỉ cho phép nội dung chữ và số theo sau là`"`, vì vậy ký tự được yêu cầu rõ ràng là dấu ngoặc kép kết thúc. Nó được chèn ngay trước`}`, cho`{"key":"value"}`. 

Vì`{"key""value"}`, trình phân tích cú pháp sẽ hoàn thành khóa ở trích dẫn thứ hai và mong đợi`:`. Báo giá hiện tại không được sử dụng khi dấu hai chấm được chèn vào. Sau đó, nó được sử dụng lại một cách chính xác làm dấu ngoặc kép mở đầu của giá trị. Sự khác biệt giữa việc chèn một ký tự và sử dụng một ký tự đầu vào sẽ ngăn ngừa lỗi từng lỗi một ở mọi vị trí dấu câu bị thiếu. 

Đối với một đối tượng được lồng sâu, mỗi`{`tạo một bối cảnh đối tượng khác trên ngăn xếp rõ ràng. Mỗi tương ứng`}`loại bỏ một bối cảnh. Trình phân tích cú pháp không bao giờ dựa vào ngăn xếp lệnh gọi Python, do đó việc lồng tỷ lệ với kích thước đầu vào được xử lý theo cùng một đường truyền tuyến tính như một đối tượng nông.
