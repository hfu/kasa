require 'json'
CONFIG = JSON.parse(`parse-hocon global.conf`)

def create_list
  list = []
  File.foreach(CONFIG['LIST_PATH']) {|line|
    list << line.strip.split(',').map {|v| v.to_i}
  }
  list
end

task :produce do
  w = File.open('list.csv', 'w')
  list = create_list
  CONFIG['MINZOOM'].upto(CONFIG['MAXZOOM'] - 1) {|z|
    list.each {|r|
      next unless r[0] == z
      s = [r.join('-')]
      2.times {|dx|
        2.times {|dy|
          t = [r[0] + 1, r[1] * 2 + dx, r[2] * 2 + dy]
          s << t.join('-') if list.index(t)
        }
      }
      w.print s.join(','), "\n"
      $stderr.print s.join(','), "\n"
    }
  }
  w.close
end

