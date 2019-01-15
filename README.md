# Parsecsv    
## Description    
*read  
*output  
## Code  
<?php

class Parsecsv
{
    public function read($path, $header = null)
    {
        /*
        * Make csv file to array
        */
        $fp = fopen($path, 'r');
        $i = 0;
        while (($line = fgetcsv($fp)) !== false) {
            if (empty($line)) {
                continue;
            }
            mb_convert_variables('UTF-8', 'SJIS', $line);
            if ($header == null) {
                $data[] = $line;
            } elseif ($i === 0) {
                $header = $line;
            } else {
                $data[] = array_combine($header, $line);
            }
            ++$i;
        }
        fclose($fp);

        return $data;
    }

    public function output($data, $path = 'no.csv', $header = null, $download = null, $encode = 'SJIS')
    {
        /*
        * Make array to csv file
        * option: header,download
        */
        $fp = fopen($path, 'w');
        if ($header) {
            fputcsv($fp, mb_convert_encoding($header,$encode, 'auto'));
        }
        foreach ($data as $val) {
            fputcsv($fp, mb_convert_encoding($val,$encode, 'auto'));
        }
        fclose($fp);
        if ($download):
            header('Content-Type: application/force-download');
            header('Content-Length: '.filesize($path));
            header('Content-disposition: attachment; filename="'.$path.'"');
        readfile($path);
        endif;
    }
}

  
## Author    
[camelG](https://github.com/camelG)  